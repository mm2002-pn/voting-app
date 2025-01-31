pipeline {
    agent any

    environment {
        REGISTRY = 'your_dockerhub_username'
        IMAGE_NAME = 'voting-app'
        VERSION = 'latest'
    }

    stages {
        stage('Build Images') {
            steps {
                script {
                    // Construire les images Docker pour chaque service
                    sh 'docker-compose build'
                }
            }
        }

        stage('Push Images to Docker Hub') {
            steps {
                script {
                    // Tagger et pousser les images
                    sh "docker tag voting-app_vote ${REGISTRY}/${IMAGE_NAME}_vote:${VERSION}"
                    sh "docker tag voting-app_result ${REGISTRY}/${IMAGE_NAME}_result:${VERSION}"
                    sh "docker tag voting-app_worker ${REGISTRY}/${IMAGE_NAME}_worker:${VERSION}"

                    sh "docker push ${REGISTRY}/${IMAGE_NAME}_vote:${VERSION}"
                    sh "docker push ${REGISTRY}/${IMAGE_NAME}_result:${VERSION}"
                    sh "docker push ${REGISTRY}/${IMAGE_NAME}_worker:${VERSION}"
                }
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                script {
                    // Démarrer les conteneurs via Docker Compose
                    sh 'docker-compose up -d'
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
