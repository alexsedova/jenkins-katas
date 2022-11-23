pipeline {
  agent any
  stages {
    stage ('Clone down') {
      agent {
        label 'master-label'
      }
      steps {
        stash excludes: '.git/', name: 'code'
      }
    }
    stage('Parallel ') {
      parallel {
        stage('Say hello') {
          steps {
            sh 'echo \'Hello\''
          }
        }
        stage('build app') {
          agent {
            docker {
              image 'gradle:6-jdk11'
            }
          }
          options {
            skipDefaultCheckout true
          }
          steps {
            unstash name: 'code'
            sh 'ci/build-app.sh'
            archiveArtifacts 'app/build/libs/'
            sh 'ls -la'
            deleteDir()
            sh 'ls -la'
          }
        }

      }
    }

  }
}
