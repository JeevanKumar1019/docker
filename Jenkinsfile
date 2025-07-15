pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-id') // Jenkins credentials ID
        DOCKER_IMAGE = 'your_dockerhub_username/flask-docker-app'
    }

    stages {
        stage('Clone Code') {
            steps {
                git url: 'https://github.com/JeevanKumar1019/docker.git', branch:'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}")
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('', "${DOCKERHUB_CREDENTIALS}") {
                        echo 'Logged in to Docker Hub'
                    }
                }
            }
        }

        stage('Push Image') {
            steps {
                script {
                    docker.withRegistry('', "${DOCKERHUB_CREDENTIALS}") {
                        docker.image("${DOCKER_IMAGE}").push()
                    }
                }
            }
        }

        stage('Deploy Container') {
            steps {
                sh 'docker run -d -p 5000:5000 ${DOCKER_IMAGE}'
            }
        }
    }
}
