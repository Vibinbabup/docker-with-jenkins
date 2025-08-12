pipeline {
    agent any

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Vibinbabup/docker-with-jenkins.git',
                    credentialsId: 'github-token'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t vibin0104/myapp:latest .'
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub_credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'docker login -u "$DOCKER_USER" -p "$DOCKER_PASS"'
                    sh 'docker push vibin0104/myapp:latest'
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                sh 'docker rm -f myapp-container || true'
                sh 'docker run -d -p 8080:8080 --name myapp-container vibin0104/myapp:latest'
            }
        }
    }

    post {
        success {
            emailext(
                to: 'vibin0104@gmail.com',
                subject: "✅ SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """\
Good news! The build succeeded.

Job: ${env.JOB_NAME}
Build Number: ${env.BUILD_NUMBER}
URL: ${env.BUILD_URL}
"""
            )
        }
        failure {
            emailext(
                to: 'vibin0104@gmail.com',
                subject: "❌ FAILURE: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """\
Oops! The build failed.

Job: ${env.JOB_NAME}
Build Number: ${env.BUILD_NUMBER}
URL: ${env.BUILD_URL}
"""
            )
        }
    }
}
