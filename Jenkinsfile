pipeline {
    agent pipeline {
    agent { label 'pipeline' }// Or 'any' if running directly on your SSH node

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Jenin011/python_docker.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'sudo docker build -t python-docker-app .'
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                    sudo docker rm -f my-python-app || true
                    sudo docker run -d -p 8000:5005 --restart unless-stopped --name my-python-app python-docker-app
                '''
            }
        }
    }

    post {
        success {
            mail to: 'kjenin48@gmail.com',
                 subject: "SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: "New changes were deployed to http://98.93.216.88:8000\nBuild URL: ${env.BUILD_URL}"
        }
        failure {
            mail to: 'kjenin48@gmail.com',
                 subject: "FAILED: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: "Deployment failed. Check logs at: ${env.BUILD_URL}console"
        }
    }
}
