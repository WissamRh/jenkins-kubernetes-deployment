pipeline {
  environment {
    dockerimagename = "wissamrh/react-app"
    dockerImage = ""
    kubeconfig = 'C:\Users\hp\.kube\config'
  }
  agent any
  stages {
    stage('Checkout Source') {
      steps {
        git branch: 'main', credentialsId: 'github-credentials', url: 'https://github.com/WissamRh/jenkins-kubernetes-deployment.git'
      }
    }
    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }
    stage('Pushing Image') {
      environment {
          registryCredential = 'dockerhub-credentials'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }
    stage('Deploying React.js container to Kubernetes') {
      steps {
        script {
          bat 'kubectl apply -f deployment.yaml'
          bat 'kubectl apply -f service.yaml'
        }
      }
    }
  }
}
