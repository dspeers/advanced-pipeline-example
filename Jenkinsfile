pipeline {
  agent any
  stages {
    stage('Say Hello') {
      steps {
        echo "Hello ${params.Name}!"
        sh 'java -version'
      }
    }
    stage('Testing') {
      failFast true
      parallel {
        stage('Java 7') {
          agent {
            docker 'openjdk:7-jdk-alpine'
          }
          steps {
            sh 'java -version'
            sleep(time: 10, unit: 'SECONDS')
          }
        }
        stage('Java 8') {
          agent {
            docker 'openjdk:8-jdk-alpine'
          }
          steps {
            sh 'java -version'
            sleep(time: 20, unit: 'SECONDS')
          }
        }
      }
    }
    stage('Deploy') {
      when {
        beforeAgent true
        branch 'master'
      }
      steps {
        timeout(time: 5, unit: 'MINUTES') {
          input(message: 'Approve Deployment?', id: 'deploy', ok: 'Approved')
        }
        
        echo "Deploying ${APP_VERSION}."
      }
    }
  }
  parameters {
    string(name: 'Name', defaultValue: 'whoever you are', description: 'Who should I say hi to?')
  }
}