pipeline {
  agent any

  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds-id') // Jenkins credentials
    DOCKERHUB_USERNAME = 'tharikashree'
    IMAGE_CLIENT = "${DOCKERHUB_USERNAME}/client"
    IMAGE_SERVER = "${DOCKERHUB_USERNAME}/server"
  }

  stages {
    stage('Clone Repo') {
      steps {
        git 'https://your-git-repo.git'
      }
    }

    stage('Build Docker Images') {
      steps {
        script {
          docker.build("${IMAGE_CLIENT}", './client')
          docker.build("${IMAGE_SERVER}", './server')
        }
      }
    }

    stage('Push to Docker Hub') {
      steps {
        script {
          docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-creds-id') {
            docker.image("${IMAGE_CLIENT}").push('latest')
            docker.image("${IMAGE_SERVER}").push('latest')
          }
        }
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        script {
          sh 'kubectl apply -f k8s/client-deployment.yaml'
          sh 'kubectl apply -f k8s/client-service.yaml'
          sh 'kubectl apply -f k8s/server-deployment.yaml'
          sh 'kubectl apply -f k8s/server-service.yaml'
        }
      }
    }
  }
}
