
pipeline {
  agent any

  tools {
    maven 'Maven'
  }

  stages {
    stage('initialize') {
      steps {
        sh 'mvn -v'
        sh 'java -version'
        sh 'git --version'
        sh 'docker -v'
      }
    }

    stage('build') {
      steps {
        checkout([$class: 'GitSCM', branches: [[name: '*/dev']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Kasutu/demo.git']]])
        sh 'mvn clean install'
      }
    }

    stage('build docker image') {
      steps {
        script{
          sh 'docker build -t kasutu/spring-test .'
        }
      }
    }

    stage('deploy') {
      steps {
        script {
          withCredentials([string(credentialsId: 'docker-credentials-kasutu', variable: 'docker-credentials')]) {
            sh "docker login -u kasutu -p ${docker-credentials}"
          }

          sh "docker push kasutu/spring-test:latest"
        }
      }
    }
  }
}
