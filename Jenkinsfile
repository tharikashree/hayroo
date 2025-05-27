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
            script {
                def result = bat(script: 'curl -s --head https://index.docker.io/v1/ | head -n 1', returnStatus: true)
                if (result != 0) {
                    error("❌ Docker Hub is not reachable")
                } else {
                    echo "✅ Docker Hub is reachable"
                }
            }
        }
    }
    stage('Push to Docker Hub') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub-creds-id', usernameVariable: 'DOCKERHUB_USERNAME', passwordVariable: 'DOCKERHUB_PASSWORD')]) {
          bat '''
            echo $DOCKERHUB_PASSWORD | docker login -u $DOCKERHUB_USERNAME --password-stdin
            docker push ${DOCKERHUB_USERNAME}/client:latest
            docker push ${DOCKERHUB_USERNAME}/server:latest
          '''
        }
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        script {
          bat 'kubectl apply -f k8s/client-deployment.yaml'
          bat 'kubectl apply -f k8s/client-service.yaml'
          bat 'kubectl apply -f k8s/server-deployment.yaml'
          bat 'kubectl apply -f k8s/server-service.yaml'
        }
      }
    }
  }
}
