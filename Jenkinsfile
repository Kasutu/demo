
pipeline {
  agent any

  tools {
    maven 'Maven'
  }

  environment {
    USERNAME = 'kasutu'
  }


  stages {
    stage('initialize') {
      when { environment name: 'initialize', value: 'true' }

      steps {
        sh 'mvn -v'
        sh 'java -version'
        sh 'git --version'
        sh 'docker -v'
      }
    }

    stage('build') {
      when { environment name: 'build', value: 'true' }

      steps {
        checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Kasutu/demo.git']]])
        sh 'mvn clean install'
      }
    }

    stage('build docker image') {
      when { environment name: 'buildDockeImage', value: 'true' }

      steps {
        script {
          sh "docker build -t ${USERNAME}/spring-test ."
        }
      }
    }

    stage('deploy') {
      when { environment name: 'deploy', value: 'true' }

      steps {
        script {
          withCredentials([usernamePassword(credentialsId: 'docker-pwd', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
            sh "docker login -u ${USERNAME} -p ${PASSWORD}"
          }
         

          sh "docker push ${USERNAME}/spring-test:latest"
        }
      }
    }
  }
}
