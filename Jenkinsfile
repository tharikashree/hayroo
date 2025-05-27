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
        git url: 'https://github.com/tharikashree/hayroo.git', branch: 'main'
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

    stage('Check Docker Hub Reachability') {
      steps {
        sh 'curl -v https://registry-1.docker.io/v2/'
      }
    }
    stage('Push to Docker Hub') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub-creds-id', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
          sh '''
            echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
            docker push ${DOCKER_USER}/client:latest
            docker push ${DOCKER_USER}/server:latest
          '''
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
