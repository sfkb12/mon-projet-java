pipeline {
    agent { label 'vm-a-tout-faire' }
    
    tools {
        maven 'M3'
    }

    environment {
        IMG="mon-projet-java-mathieu:${env.BUILD_NUMBER}"
        CT_NAME="mon-projet-java-mathieu-container"
        URL_NOTIFICATIONS="https://ntfy.sh/1x6DHZYwBpRxKUJF"
    }

    stages {
        stage('Compilation du projet') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build docker image') {
            steps {
                sh "docker build -t ${IMG} ."
            }
        }

        stage('deploiement') {
            steps {
                sh "docker stop ${CT_NAME} || true"
                sh "docker rm ${CT_NAME} || true"
                sh "docker run -d --name ${CT_NAME} ${IMG}"
            }
        }
    }

    post {
        success {
            script {
                echo "Ca a fonctionné"
                sh "docker ps | grep ${CT_NAME}"
                sh "docker logs ${CT_NAME}"
                sh """
                curl -H "Title: Build réussi" \
                     -H "Priority: default" \
                     -H "Tags: white_checkmark,docker" \
                     -d "Notre app java a été déployée, via le job ${env.JOB_NAME} avec le build n°${env.BUILD_NUMBER}" \
                     ${URL_NOTIFICATIONS}
                """
            }
        }
        failure {
            script {
                sh """
                curl -H "Title: Build échoué" \
                     -H "Priority: high" \
                     -H "Tags: x,warning" \
                     -d "ATTENTION: le job ${env.JOB_NAME} avec le build n°${env.BUILD_NUMBER} a échoué" \
                     ${URL_NOTIFICATIONS}
                """
            }
        }
    }
}
