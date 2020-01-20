pipeline {

  environment {
    registry = "172.17.0.2:5000/myweb"
    dockerImage = ""
  }

  agent none

  stages {

    stage('Checkout Source') {
      agent { label 'kubepods' }
      steps {
        git 'https://github.com/Alikoo/playjenkins.git'
      }
    }

    stage('Build image') {
      agent { label 'kubepods' }
      steps{
        script {
          dockerImage = docker.build registry + ":latest"
        }
      }
    }

    stage('Push Image') {
      agent { label 'kubepods' }
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
