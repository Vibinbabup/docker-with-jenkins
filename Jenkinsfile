pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = "vibinbabup/myapp"
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Use correct branch and credentials if needed
                git branch: 'main', url: 'https://github.com/Vibinbabup/docker-with-jenkins.git'
            }
        }

        stage('Install Dependencies & Run Tests') {
            steps {
                sh 'pip install -r requirements.txt'
                // If you have tests, run them here
                // sh 'pytest'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE}:latest ."
            }
        }

        stage('Login to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub_credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS'_]()
