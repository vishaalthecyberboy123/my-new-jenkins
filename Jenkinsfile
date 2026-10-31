
pipeline {
    agent any

    environment {
        IMAGE_NAME = 'tfc-nation'
        CONTAINER_NAME = 'tfc-nation-container'
        HOST_PORT = '8082'
    }

    stages {

        stage('Check Source') {
            steps {
                sh 'ls -la'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${IMAGE_NAME}:latest .'
            }
        }

        stage('Remove Old Container') {
            steps {
                sh 'docker rm -f ${CONTAINER_NAME} || true'
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                    docker run -d \
                    --name ${CONTAINER_NAME} \
                    -p ${HOST_PORT}:80 \
                    ${IMAGE_NAME}:latest
                '''
            }
        }

        stage('Verify Website') {
            steps {
                sh '''
                    sleep 3
                    docker images
                    docker ps
                    curl -f -I http://localhost:${HOST_PORT}
                '''
            }
        }
    }

    post {
        success {
            echo 'TFC NATION DEPLOYMENT SUCCESSFUL!'
            echo 'Website running on port 8082'
        }

        failure {
            echo 'DEPLOYMENT FAILED!'
            sh 'docker ps -a || true'
        }
    }
}
