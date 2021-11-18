pipeline {
  environment {
    registry = "rekaforgacs/devops_coursework2"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
  agent any
  stages {
    // stage('Cloning Git') {
    //   steps {
    //     git 'https://github.com/Reekaa/DevOps_Coursework2.git'
    //   }
    // }
    stage('Building docker image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Deploy docker image to docker hub') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry:$BUILD_NUMBER"
      }
    }
  }
}