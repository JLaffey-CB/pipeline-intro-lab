pipeline {
  agent any
  stages {
    stage('Fluffy Build') {
      steps {
        sh './Jenkins/test-all.sh'
      }
    }
    stage('Fluffy Test') {
      steps {
        sh ' ./Jenkins/build.sh'
      }
    }
    stage('Fluffy Deploy') {
      steps {
        sh './Jenkins/deploy.sh staging'
      }
    }
  }
}