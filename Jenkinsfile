pipeline {
    agent any

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main',
                    credentialsId: 'github-token',
                    url: 'https://github.com/vibin0104/docker-with-jenkins.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t vibin0104/myapp .'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh 'docker run -d -p 8080:8080 --name myapp-container vibin0104/myapp'
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
