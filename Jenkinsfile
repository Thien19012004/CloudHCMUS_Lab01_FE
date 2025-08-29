pipeline {
    agent any

    environment {
        DOCKERHUB = credentials('dockerhub-cred')

        DOCKERHUB_REPO = "thien19012004/cloudhcmus_lab01_fe"

        EC2_SSH   = credentials('ec2-ssh') 
        EC2_USER  = "ubuntu"
        EC2_HOST  = "16.176.179.11"
        APP_PORT  = "2048" 

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

        stage('Deploy to EC2') {
            steps {
                sshagent (credentials: ['ec2-ssh']) {
                    sh """
                        ssh -o StrictHostKeyChecking=no ${EC2_USER}@${EC2_HOST} '
                            docker pull ${DOCKERHUB_REPO}:latest &&
                            docker stop fe-app || true &&
                            docker rm fe-app || true &&
                            docker run -d -p ${APP_PORT}:${APP_PORT} --name fe-app ${DOCKERHUB_REPO}:latest
                        '
                    """
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
