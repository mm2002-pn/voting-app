pipeline {
    agent any
    environment {
        DOCKER_IMAGE_VOTE = "diallo2910/voting-app_vote:latest"
        DOCKER_IMAGE_RESULT = "diallo2910/voting-app_result:latest"
        DOCKER_IMAGE_WORKER = "diallo2910/voting-app_worker:latest"
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/mm2002-pn/voting-app.git'
            }
        }
        stage('Build Docker Images') {
            steps {
                script {
                    sh 'docker build -t ${DOCKER_IMAGE_VOTE} ./vote'
                    sh 'docker build -t ${DOCKER_IMAGE_RESULT} ./result'
                    sh 'docker build -t ${DOCKER_IMAGE_WORKER} ./worker'
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    sh 'echo $DOCKER_PASSWORD | docker login --username $DOCKER_USERNAME --password-stdin'
                }
            }
        }


        stage('Push Docker Images to Docker Hub') {
            steps {
                script {
                    sh 'docker push ${DOCKER_IMAGE_VOTE}'
                    sh 'docker push ${DOCKER_IMAGE_RESULT}'
                    sh 'docker push ${DOCKER_IMAGE_WORKER}'
                }
            }
        }
        stage('Deploy with Docker Compose') {
            steps {
                script {
                    sh 'docker-compose down'
                    sh 'docker-compose up -d'
                }
            }
        }
    }
    post {
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo 'Deployment Failed!'
        }
    }
}
