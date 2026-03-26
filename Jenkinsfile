pipeline {
    agent { label 'vm-a-tout-faire' }
    
    tools {
        maven 'M3'
    }

    environment {
        IMG="mon-projet-java-mathieu:${env.BUILD_NUMBER}"
        CT_NAME="mon-projet-java-mathieu-container"
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
            echo "Ca a fonctionné"
            sh "docker ps | grep ${CT_NAME}"
            sh "docker logs ${CT_NAME}"
        }
    }
}
