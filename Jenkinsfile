pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'manh1310/devopslab3'
        DOCKER_TAG = 'latest'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/manhlamviectarantula/devopslab3.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}:${DOCKER_TAG}")
                }
            }
        }

        stage('Integration Tests') {
            steps {
                echo 'Running Integration tests...'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-hub-credentials') {
                        docker.image("${DOCKER_IMAGE}:${DOCKER_TAG}").push()
                    }
                }
            }
        }

        stage('Deploy Golang to DEV') {
            steps {
                echo 'Deploying to DEV...'
                sh 'docker image pull manh1310/devopslab3:latest'
                sh 'docker container stop devopslab3 || echo "this container does not exist"'
                sh 'docker network create dev || echo "this network exists"'
                sh 'echo y | docker container prune '

                sh 'docker container run -d --rm --name lab3devops -p 3000:3000 --network dev manh1310/devopslab3:latest'
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}