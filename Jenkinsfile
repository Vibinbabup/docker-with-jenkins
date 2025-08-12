pipeline {
    agent any

    environment {
        DOCKERHUB_USER = credentials('dockerhub-username') // Jenkins credential ID
        IMAGE_NAME = "vibinbabup/docker-with-jenkins"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/Vibinbabup/docker-with-jenkins.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${IMAGE_NAME}:latest ."
                }
            }
        }

        stage('Login to DockerHub') {
            steps {
                script {
                    sh "echo ${DOCKERHUB_USER_PSW} | docker login -u ${DOCKERHUB_USER_USR} --password-stdin"
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    sh "docker push ${IMAGE_NAME}:latest"
                }
            }
        }
    }

    post {
        failure {
            echo "Build failed — Docker image will not be pushed."
        }
    }
}
