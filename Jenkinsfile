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
    stage('Run docker image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.run("$image:$BUILD_NUMBER")
          }
        }
        sh 'docker ps'
      }
    }
    stage('Update deployment') {
      steps{
        withKubeConfig([credentialsId: 'kubernetes-config']) {
          sh 'curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl"'  
          sh 'chmod u+x ./kubectl'  
          sh "./kubectl set image deployments/devops-coursework2 devops-coursework2=$image:$BUILD_NUMBER"
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