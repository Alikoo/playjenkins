pipeline {

  environment {
    registry = "172.17.0.2:5000/myweb"
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
        //sh 'ping -c 3 registry'
        sh 'cat /etc/hosts'

        script {
          docker.withRegistry( "" ) {
            dockerImage.push()
          }
        }
      }
    }

    stage('Deploy App') {
      steps {
        agent { label 'master' }
        script {
          kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "kubeconfig")
        }
      }
    }

  }

}
