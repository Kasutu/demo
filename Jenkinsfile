
pipeline {
  agent any

  tools {
    maven 'Maven'
  }

  def env = System.getenv()
  def username = env['USERNAME']
  def password = env['PASSSWORD']

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
          sh "docker build -t ${username}/spring-test ."
        }
      }
    }

    stage('deploy') {
      when { environment name: 'deploy', value: 'true' }

      steps {
        script {
         
          sh "docker login -u ${username} -p ${password}"

          sh "docker push ${username}/spring-test:latest"
        }
      }
    }
  }
}
