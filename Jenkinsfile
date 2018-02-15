pipeline {
  agent any
  stages {
    stage('Build') {
      parallel {
        stage('Build') {
          steps {
            echo 'Hello World. Build Stage'
          }
        }
        stage('Buil 2 Parellel') {
          steps {
            sleep 3
          }
        }
      }
    }
    stage('Push') {
      steps {
        echo 'Pushing'
      }
    }
    stage('Test1') {
      parallel {
        stage('Test1') {
          steps {
            echo 'Testing 1'
          }
        }
        stage('Test2') {
          steps {
            echo 'Testing2'
            echo 'Testing 2 again'
          }
        }
      }
    }
    stage('deploy') {
      steps {
        echo 'Deploying'
      }
    }
  }
}