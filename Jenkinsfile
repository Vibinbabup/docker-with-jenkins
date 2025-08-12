pipeline {
    agent any

    environment {
        IMAGE_NAME = "vibin0104/jenkins-demo:latest"
    }

    stages {
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
                withCredentials([usernamePassword(credentialsId: 'dockerhub_credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                }
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
}
