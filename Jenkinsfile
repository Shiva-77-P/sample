pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Shiva-77-P/sample.git'
            }
        }

        stage('Install & Test') {
            agent {
                docker { image 'node:18-alpine' }
            }
            steps {
                sh 'npm install'
                sh 'npm test || true'
            }
        }

        stage('Build Docker Image') {
            steps {
                // This runs on the host (Jenkins server), not inside the node container
                sh 'docker build -t nodeapp:latest .'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker stop nodeapp || true'
                sh 'docker rm nodeapp || true'
                sh 'docker run -d -p 3001:3000 --name nodeapp nodeapp:latest'
            }
        }
    }
}