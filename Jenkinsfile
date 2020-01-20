pipeline {

  agent any

  environment {
    registry = "172.17.0.2:5000/alikoo/web"
    dockerImage = ""
  }

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/Alikoo/playjenkins.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build registry + ":latest"
        }
      }
    }

    stage('Push Image') {
      steps{
        script {
          docker.withRegistry( "" ) {
            dockerImage.push()
          }
        }
      }
    }
}