pipeline {

  environment {
    registry = "regisrty:5000/myweb"
    dockerImage = ""
  }

  agent { label 'kubepods' }

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

    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "kubeconfig")
        }
      }
    }

  }

}
