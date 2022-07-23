pipeline {
  environment {
    registry = "lakshitsainiceligo/node.js-hello-world-microservice-example"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
  agent any
  tools {nodejs "18.6.0"}
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/lakshitsaini/Node.js-Hello-World-Microservice-Example.git'
      }
    }
    stage('Build') {
       steps {
         sh 'npm install'
       }
    }
    stage('Test') {
      steps {
        sh 'npm test'
      }
    }
    stage('Initialize') {
        steps{
            script{
                def dockerHome = tool 'docker'
                env.PATH = "${dockerHome}/bin:${env.PATH}"
            }
        }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build("lakshitsainiceligo/node.js-hello-world-microservice-example")
        }
      }
    }
    stage('Deploy Image') {
      steps{
         script {
            docker.withRegistry('https://registry.hub.docker.com', registryCredential ) {
            //dockerImage.push()
            dockerImage.push("${env.BUILD_NUMBER}")            
            dockerImage.push("latest")  
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
