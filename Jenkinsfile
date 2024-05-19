pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/rajeev3012/devops-project.git'
        BRANCH_NAME = 'main'
        DOCKER_IMAGE = 'my-app'
        DOCKER_TAG = 'latest'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: "${BRANCH_NAME}", url: "${REPO_URL}"
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("my-app:latest")
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Stop and remove any existing container to avoid port conflicts
                    sh 'docker rm -f my-app-container || true'

                    // Run the Docker container
                    docker.image("${DOCKER_IMAGE}:${DOCKER_TAG}").run('-d -p 5000:5000 --name my-app-container')
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-hub-creds') {
                        docker.image("${DOCKER_IMAGE}:${DOCKER_TAG}").push()
                    }
                }
            }
        }
    }

    post {
        always {
            script {
                // Clean up Docker images
                sh 'docker rmi ${DOCKER_IMAGE}:${DOCKER_TAG} || true'
                sh 'docker rmi ${DOCKER_IMAGE} || true'
            }
            echo "Clean up completed"
        }
    }
}
