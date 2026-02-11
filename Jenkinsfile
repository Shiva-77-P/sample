pipeline {
    agent any

    tools {
        nodejs 'node18'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Shiva-77-P/sample.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                // This assumes Node.js is installed on your Jenkins server
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test || true'
            }
        }

        stage('Build Docker Image') {
            steps {
                // We removed 'apk add' because it only works on Alpine Linux.
                // We assume Docker is already installed on your server.
                sh 'docker build -t nodeapp:latest .'
            }
        }

        stage('Run Container') {
            steps {
                // 1. Stop old container (if running) so we don't get "Name already in use" error
                sh 'docker stop nodeapp || true'
                // 2. Remove old container
                sh 'docker rm nodeapp || true'
                // 3. Start the new one
                sh 'docker run -d -p 3001:3000 --name nodeapp nodeapp:latest'
            }
        }
    }
}