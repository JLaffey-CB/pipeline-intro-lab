Here is the solution `Jenkinsfile` for Section 4:

    pipeline {
      agent any
      stages {
        stage('Fluffy Build') {
          parallel {
            stage('Build Java 7') {
              steps {
                sh './jenkins/build.sh'
                archiveArtifacts 'target/*.jar'
                stash(name: 'Java 7', includes: 'target/**')
              }
            }
            stage('Build Java 8') {
              steps {
                sh './jenkins/build.sh'
                stash(name: 'Java 8', includes: 'target/**')
              }
            }
          }
        }
        stage('Fluffy Test') {
          parallel {
            stage('BackendJava7') {
              agent {
                node {
                  label 'java7'
                }

              }
              steps {
                unstash 'Java 7'
                sh './jenkins/test-backend.sh'
                junit 'target/surefire-reports/**/TEST*.xml'
              }
            }
            stage('FrontendJava7') {
              agent {
                node {
                  label 'java7'
                }

              }
              steps {
                sh './jenkins/test-frontend.sh'
                junit 'target/test-results/**/TEST*.xml'
              }
            }
            stage('PerformanceJava7') {
              agent {
                node {
                  label 'java7'
                }

              }
              steps {
                sh './jenkins/test-performance.sh'
              }
            }
            stage('StaticJava7') {
              agent {
                node {
                  label 'java7'
                }

              }
              steps {
                sh './jenkins/test-static.sh'
              }
            }
            stage('BackendJava8') {
              agent {
                node {
                  label 'java8'
                }

              }
              steps {
                unstash 'Java 8'
                sh './jenkins/test-backend.sh'
                junit 'target/surefire-reports/**/TEST*.xml'
              }
            }
            stage('FrontendJava8') {
              agent {
                node {
                  label 'java8'
                }

              }
              steps {
                sh './jenkins/test-frontend.sh'
                junit 'target/test-results/**/TEST*.xml'
              }
            }
            stage('PerformanceJava8') {
              agent {
                node {
                  label 'java8'
                }

              }
              steps {
                sh './jenkins/test-performance.sh'
              }
            }
            stage('StaticJava8') {
              agent {
                node {
                  label 'java8'
                }

              }
              steps {
                sh './jenkins/test-static.sh'
              }
            }
          }
        }
        stage('Confirm Deploy') {
          steps {
            checkpoint 'Ready to Deploy'
            input(message: 'Is the build okay to deploy?', ok: 'Yes')
          }
        }
        stage('Fluffy Deploy') {
          agent {
            node {
              label 'java7'
            }

          }
          steps {
            unstash 'Java 7'
            sh './jenkins/deploy.sh staging'
          }
        }
      }
    }
