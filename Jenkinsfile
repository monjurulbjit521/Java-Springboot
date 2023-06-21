pipeline {
  agent any

  tools {
    maven 'maven'
    jdk 'JDK17'
  }

  stages {
    stage('Build Maven') {
      steps {
        sh 'mvn clean install package'
      }
    }
  
    stage('Copy Artifact') {
      steps {
        sh 'cp -r target/*.jar docker'
      }
    }

    stage('Build Docker Image') {
      steps {
        script {
          def customImage = docker.build('monjurul12/spring', './docker')
          docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            customImage.push("${env.BUILD_NUMBER}")
          }
        }
      }
    }
  }
}
