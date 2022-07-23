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
                RUN curl -fsSLO https://get.docker/builds/Linux/x86_64/docker-17.04.0-ce.tgz \
                                      && tar xzvf docker-17.04.0-ce.tgz \
                                      && mv docker/docker /usr/local/bin \
                                      && rm -r docker docker-17.04.0-ce.tgz
            }
        }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Deploy Image') {
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
