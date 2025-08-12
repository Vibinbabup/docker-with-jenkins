pipeline {
    agent any

    environment {
        IMAGE_NAME = "vibin0104/jenkins-demo:latest"
    }

    stages {
        stage('Install Dependencies & Run Tests') {
            steps {
                echo "Installing dependencies..."
                sh 'pip install -r requirements.txt'

                echo "Running unit tests..."
                // This will fail the build if any test fails
                sh 'python -m unittest discover -v'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image..."
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }

        stage('Login to DockerHub') {
            steps {
                echo "Logging in to DockerHub..."
                withCredentials([usernamePassword(credentialsId: 'dockerhub_credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                echo "Pushing image to DockerHub..."
                sh "docker push ${IMAGE_NAME}"
            }
        }

        stage('Cleanup') {
            steps {
                echo "Cleaning up local Docker image..."
                sh "docker rmi ${IMAGE_NAME} || true"
            }
        }
    }

    post {
        failure {
            echo "Build failed — Docker image will not be pushed."
        }
        success {
            echo "Build and push successful."
        }
    }
}
