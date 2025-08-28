pipeline {
    agent any

    environment {
        DOCKERHUB = credentials('dockerhub-cred')

        DOCKERHUB_REPO = "thien19012004/cloudhcmus_lab01_fe"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Thien19012004/CloudHCMUS_Lab01_FE.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${DOCKERHUB_REPO}:latest ."
                }
            }
        }

        stage('DockerHub Login') {
            steps {
                script {
                    sh "echo ${DOCKERHUB_PSW} | docker login -u ${DOCKERHUB_USR} --password-stdin"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    sh "docker push ${DOCKERHUB_REPO}:latest"
                }
            }
        }
    }

    post {
        success {
            echo "Build & Push Docker image thành công!"
        }
        failure {
            echo " Có lỗi xảy ra. Kiểm tra logs."
        }
    }
}
