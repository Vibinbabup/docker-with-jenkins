pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Vibinbabup/docker-with-jenkins.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("vibinbabup/docker-with-jenkins")
                }
            }
        }

        stage('Login to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub_credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    dockerImage.push("latest")
                }
            }
        }
    }

    post {
        failure {
            echo 'Build failed — Docker image will not be pushed.'
        }
        success {
            echo 'Build succeeded — Docker image pushed to DockerHub.'
        }
    }
}
