pipeline {
  environment {
    image = "rekaforgacs/devops_coursework2"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Building docker image') {
      steps{
        script {
          dockerImage = docker.build image
        }
      }
    }
    stage('Deploy docker image to docker hub') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push("$BUILD_NUMBER")
            dockerImage.push('latest')
          }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $image:$BUILD_NUMBER"
      }
    }
  }
}