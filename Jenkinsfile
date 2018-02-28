pipeline {
  agent any
  stages {
    stage('Fluffy Build') {
      steps {
        sh './jenkins/build.sh'
      }
    }
    stage('Fluffy Test') {
      steps {
        sh ' ./jenkins/test-all.sh'
      }
    }
    stage('Fluffy Deploy') {
      steps {
        sh './jenkins/deploy.sh staging'
      }
    }
  }
}