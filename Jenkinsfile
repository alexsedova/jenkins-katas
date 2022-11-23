pipeline {
  agent any
  stages {
    stage('Clone down') {
      agent {
        label 'swarm'
      }
      steps {
        stash name: 'code', excludes: '.git/'
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
          steps {
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