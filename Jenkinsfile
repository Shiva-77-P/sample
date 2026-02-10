pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/Shiva-77-P/sample.git'
      }
    }

    stage('Install Dependencies') {
      steps {
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
        sh 'docker build -t nodeapp:latest .'
      }
    }

    stage('Run Container') {
      steps {
        sh 'docker run -d -p 3001:3000 --name nodeapp nodeapp:latest || true'
      }
    }
  }
}
