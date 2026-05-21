pipeline {
    agent any

    environment {
        IMAGE_NAME = 'ci-cd-test'
        CONTAINER_NAME = 'ci-cd-test-container'
    }
    
    stages{
        stage('Clone') {
            steps { 
                git branch: 'main', url: 'https://github.com/deekshi-1/ci-cd-practice.git'
            }
        }
        stage('Build') {
            steps {
                sh 'echo "Building stage runs here"'
                sh 'docker build -t ${IMAGE_NAME} .'
            }
        }
        stage('Test') {
            steps {
                sh '''
                    echo "Testing stage runs here"
                    docker run -d --name test -p 5001:5000 ${IMAGE_NAME}
                    sleep 10
                    docker ps
                    docker logs test
                    docker stop test
                    docker rm -f test
                '''
            }
        }
        stage('Deploy') {
            steps {
                sh '''
                    echo "Building stage runs here"
                    docker run -d \
                    --name ${CONTAINER_NAME} \
                    -p 5000:5000 \
                    ${IMAGE_NAME}
                    sleep 300
                    docker stop ${CONTAINER_NAME}
                    docker rm -f ${CONTAINER_NAME}
                '''
            }
        }
    }
}
