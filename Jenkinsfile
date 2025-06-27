pipeline {
  agent any

  environment {
    DOCKERHUB_CREDENTIALS = 'dockerhub-credentials'      // Jenkins Credentials ID
    DOCKERHUB_USERNAME = 'syed411'                       // Your Docker Hub username
    IMAGE_NAME = 'docker-demo'                           // Image name
    IMAGE_TAG = 'v1'                                      // Image tag
  }

  stages {
    stage('Clone Git Repository') {
      steps {
         git branch: 'main', url: 'https://github.com/Syedshakeel23/docker.git' // Replace with your repo URL
      }
    }

    stage('Build Docker Image') {
      steps {
        script {
          echo "Building Docker image..."
          sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
        }
      }
    }

    stage('Tag Docker Image') {
      steps {
        script {
          echo "Tagging image for Docker Hub..."
          sh "docker tag ${IMAGE_NAME}:${IMAGE_TAG} ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:${IMAGE_TAG}"
        }
      }
    }

    stage('Push Docker Image to Docker Hub') {
      steps {
        withCredentials([usernamePassword(credentialsId: "${DOCKERHUB_CREDENTIALS}", usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
          script {
            echo "Logging in to Docker Hub..."
            sh "echo $PASSWORD | docker login -u $USERNAME --password-stdin"
            echo "Pushing image to Docker Hub..."
            sh "docker push ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:${IMAGE_TAG}"
          }
        }
      }
    }
  }

  post {
    success {
      echo '✅ Docker image built and pushed successfully!'
    }
    failure {
      echo '❌ Pipeline failed.'
    }
  }
}
