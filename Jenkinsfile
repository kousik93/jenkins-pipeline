pipeline {
  agent any
  stages {
    stage('Build') {
      parallel {
        stage('Build 1 Parallel') {
          steps {
            echo 'Hello World. Build Stage 1'
          }
        }
        stage('Build 2 Parellel') {
          steps {
            sleep 3
            echo 'Hello World. Build Stage 2'
          }
        }
        stage('Build 3 Parallel') {
          steps {
            echo 'Hello World. Build Stage 3'
          }
        }
      }
    }
    stage('Push') {
      steps {
        echo 'Pushing Build'
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
        stage('Test3') {
          steps {
            error 'Test 3 failed!'
          }
        }
        stage('Test4') {
          steps {
            catchError() {
              echo 'Test4 Child Step'
            }
            
          }
        }
        stage('Test5') {
          steps {
            sh '''echo "Running test5. Value of test5 env is:"
echo ${test5}
if [ ${test5} == "true" ]
then
   echo "Test5 passed"
else
   exit 42
fi'''
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
  environment {
    test5 = 'false'
  }
}