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
          post {
                          always {
                              echo 'I will always say Hello again! - From Test 3'
                          }
                          failure {
                              echo "Test 3 failed! "
                              export TEST3_RESULT= "false"
                          }
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
          post {
                                  always {
                                      echo 'I will always say Hello again! - From Test 5'
                                  }
                                  failure {
                                      echo "Test 5 failed! "
                                      export TEST5_RESULT= "false"
                                      echo ${TEST5_RESULT}
                                  }
                              }
        }

      }
    }
    stage('deploy') {
    when { environment name: 'TEST5_RESULT', value: 'true' }
      steps {
        echo 'Deploying'
      }
    }
  }
  environment {
    test5 = 'true'
    TEST1_RESULT = 'true'
    TEST2_RESULT = 'true'
    TEST3_RESULT = 'true'
    TEST4_RESULT = 'true'
    TEST5_RESULT = 'true'
  }
}