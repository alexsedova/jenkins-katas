pipeline {
  agent any
  stages {
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
          steps {
            sh 'ci/build-app.sh'
          }
        }

      }
    }

  }
}