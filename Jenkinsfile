
pipeline {
  agent any

  tools {
    maven 'Maven'
  }

  environment {
   initialize = true
   build = true
   buildDockeImage = true
   deploy = true
  }

  stages {
    stage('initialize') {
      when {expression {"${initialize}" == 'true'}}

      steps {
        sh 'mvn -v'
        sh 'java -version'
        sh 'git --version'
        sh 'docker -v'
      }
    }

    stage('build') {
      when {expression {"${build}" == 'true'}}

      steps {
        checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Kasutu/demo.git']]])
        sh 'mvn clean install'
      }
    }

    stage('build docker image') {
      when {expression {"${buildDockeImage}" == 'true'}}

      steps {
        script{
          sh 'docker build -t kasutu/spring-test .'
        }
      }
    }

    stage('deploy') {
      when {expression {"${deploy}" == 'true'}}
      
      steps {
        script {
          sh "docker push kasutu/spring-test:latest"
        }
      }
    }
  }
}
