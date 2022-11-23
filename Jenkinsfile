pipeline {
  agent any
  environment {
    docker_username = 'alesed'  
  }      
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
          }
        }

      }
    }
    stage('Push to docker') {
        environment {
           DOCKERCREDS = credentials('docker-login') //use the credentials just created in this stage
        }
        steps {
          unstash 'code' //unstash the repository code
          sh 'ci/build-docker.sh'
          sh 'echo "$DOCKERCREDS_PSW" | docker login -u "$DOCKERCREDS_USR" --password-stdin' //login to docker hub with the credentials above
          sh 'ci/push-docker.sh'
        }
    }

  }
}
