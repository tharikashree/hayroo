pipeline {
  agent any

  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds-id') // Jenkins credentials
    DOCKERHUB_USERNAME = 'tharikashree'
    IMAGE_CLIENT = "${DOCKERHUB_USERNAME}/client"
    IMAGE_SERVER = "${DOCKERHUB_USERNAME}/server"
    KUBECONFIG = "${WORKSPACE}\\.kube\\config"
  }

  stages {
    stage('Clone Repo') {
      steps {
        git url: 'https://github.com/tharikashree/hayroo.git',branch: 'main'
      }
    }

    stage('Build Docker Images') {
      steps {
        script {
          docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-creds-id') {
            docker.build("${IMAGE_CLIENT}", './client')
            docker.build("${IMAGE_SERVER}", './server')
          }
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
      stage('Inject kubeconfig securely') {
          steps {
              withCredentials([file(credentialsId: 'kubeconfig-secret', variable: 'KUBECONFIG_FILE')]) {
                  powershell '''
                      New-Item -ItemType Directory -Force -Path ".kube"
                      Copy-Item $env:KUBECONFIG_FILE ".kube\\config" -Force
                  '''
              }
          }
      }
    stage('Deploy to Kubernetes') {
      steps {
        script {
          powershell 'kubectl apply -f k8s/client-deployment.yaml'
          powershell 'kubectl apply -f k8s/client-service.yaml'
          powershell 'kubectl apply -f k8s/server-deployment.yaml'
          powershell 'kubectl apply -f k8s/server-service.yaml'
        }
      }
    }
  }
}
