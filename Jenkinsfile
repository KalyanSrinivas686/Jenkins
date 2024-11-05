pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "your-image-name" // Replace with your Docker image name
        DOCKER_REGISTRY_CREDENTIALS_ID = 'docker-hub-credentials' // Jenkins credentials ID
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/your-repo-url.git'
            }
        }
        stage('Test') {
            steps {
                sh 'npm run test' // Adjust for testing commands in your project
            }
        }
        stage('Build') {
            steps {
                sh 'npm run build' // Adjust for build commands in your project
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }
        stage('Push Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: DOCKER_REGISTRY_CREDENTIALS_ID, passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    sh "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"
                    sh "docker push $DOCKER_IMAGE"
                }
            }
        }
        stage('Deploy') {
            steps {
                sh 'kubectl apply -f deployment.yaml' // Adjust for Kubernetes, if using Kubernetes
            }
        }
    }
}
