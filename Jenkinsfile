pipeline {
    agent any

    environment {
        DOCKER_CREDS = credentials('dockerhub_credentials')
        IMAGE_NAME = "vibin0104/jenkins-demo:latest"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/Vibinbabup/docker-with-jenkins.git'
            }
        }

        stage('Install Dependencies & Run Tests') {
            steps {
                sh 'pip install -r requirements.txt'
                sh 'python -m unittest discover'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }

        stage('Login to DockerHub') {
            steps {
                sh "echo ${DOCKER_CREDS_PSW} | docker login -u ${DOCKER_CREDS_USR} --password-stdin"
            }
        }

        stage('Push to DockerHub') {
            steps {
                sh "docker push ${IMAGE_NAME}"
            }
        }

        stage('Cleanup') {
            steps {
                sh "docker rmi ${IMAGE_NAME}"
            }
        }
    }

    post {
        failure {
            echo "Build failed — Docker image will not be pushed."
        }
    }
}
