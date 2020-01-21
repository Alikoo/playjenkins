pipeline {

  agent {
        docker { image 'joao29a/jnlp-slave-alpine-docker' }
    } 

  environment {
    registry = "172.19.0.2:5000/alikoo/myweb"
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

     stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "kubeconfig")
        }
      }
    }
  }
}