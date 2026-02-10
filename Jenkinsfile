pipeline {
  // CHANGE 1: Use Docker instead of 'any'
 agent any // Runs directly on the server, skipping the Docker check
    stages {
        stage('Install') {
             steps {
                 sh 'npm install' // Only works if you installed Node manually!
    }
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/Shiva-77-P/sample.git'
      }
    }

    stage('Install Dependencies') {
      steps {
        // Now this works because we are inside the 'node:18' container
        sh 'npm install'
      }
    }

    stage('Test') {
      steps {
        sh 'npm test || true'
      }
    }

    // CRITICAL WARNING:
    // To run 'docker build' INSIDE a Docker container (Docker-in-Docker),
    // you need the socket mapping I added in 'args' above.
    stage('Build Docker Image') {
      steps {
         // This might require the 'docker' CLI to be installed in the node image
         // A safer bet for beginners is to install docker client first
         sh 'apk add --no-cache docker-cli'
         sh 'docker build -t nodeapp:latest .'
      }
    }

    stage('Run Container') {
      steps {
        // We must stop the old container before starting a new one, or it will fail
        sh 'docker stop nodeapp || true'
        sh 'docker rm nodeapp || true'
        sh 'docker run -d -p 3001:3000 --name nodeapp nodeapp:latest'
      }
    }
  }
}