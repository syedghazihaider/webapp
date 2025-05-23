pipeline {
    agent any
    environment {
        DOCKER_IMAGE ="gbilgrami/webapp"
        IMAGE_TAG = "1.0.${env.BUILD_NUMBER}"
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}:${IMAGE_TAG}")
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                withDockerRegistry(credentialsId: 'dockerhub-creds', url: '') {
                    script {
                        docker.image("${DOCKER_IMAGE}:${IMAGE_TAG}").push()
                    }
                }
            }
        }
        stage('Deploy to Kubernetes') {
  steps {
    sh """
    /usr/local/bin/kubectl set image deployment/webapp-deployment webapp=${DOCKER_IMAGE}:${IMAGE_TAG} --record
    """
  }
}


