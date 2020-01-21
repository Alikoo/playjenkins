pipeline {

  agent {
        label 'ubuntu-slave-1'
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
          dockerImage = docker.build registry + ":${currentBuild.number}"
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
        sh "java -version"
        script {
          kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "kubeconfig")
        }
      }
    }
  }
}