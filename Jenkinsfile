pipeline {
    agent any

    environment {
        IMAGE_NAME = 'mkxdesu/jenkins-pipeline'
        REGISTRY_CREDENTIALS = 'dockerhub-credentials'
        DOCKER = 'C:\\Users\\USER\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                bat 'echo "Build di Windows"'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat "\"%DOCKER%\" build -t %IMAGE_NAME%:%BUILD_NUMBER% ."
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "${REGISTRY_CREDENTIALS}",
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    bat """
                        \"%DOCKER%\" login -u %USER% -p %PASS%
                        \"%DOCKER%\" push %IMAGE_NAME%:%BUILD_NUMBER%
                        \"%DOCKER%\" tag %IMAGE_NAME%:%BUILD_NUMBER% %IMAGE_NAME%:latest
                        \"%DOCKER%\" push %IMAGE_NAME%:latest
                    """
                }
            }
        }
    }
}
