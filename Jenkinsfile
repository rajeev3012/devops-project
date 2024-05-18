pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/rajeev3012/devops-project.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build('my-app')
                }
            }
        }
        stage('Run Docker Container') {
            steps {
                script {
                    docker.image('my-app').run('-p 5000:5000')
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-hub-creds') {
                        docker.image('my-app').push('latest')
                    }
                }
            }
        }
    }
}

