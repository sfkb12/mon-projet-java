pipeline {
    agent any
    
    tools {
        maven 'M3'
    }

    stages {
        stage('Compilation') {
            steps {
                sh 'mvn clean compile'
            }
        }
    }
}
