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
      }
    }
    stage('Push') {
      steps {
        echo 'Pushing Build'
      }
    }
    stage('Test') {
      parallel {
        stage('Test2') {
          steps {
            catchError() {
                echo 'Hello 2'
            }
          }
          post {

            failure {
                echo "test2 failed"
            }
            success{
                echo "test2 success"
            }
          }
        }
        stage('Test3') {
          steps {
            catchError() {
                error 'Test 3 failed!'
            }
          }
          post {
            always {
               echo 'I will always say Hello again! - From Test 3'
            }
            failure {
                script {
                    env.TEST3_RESULT = "false"
                }
               sh '''
               echo "Test 3 failed! "
               echo "Test 3 result is::"
               echo ${TEST3_RESULT}
               '''
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
          catchError() {
            sh '''echo "Running test5. Value of test5 env is:"
                              echo ${test5}
                              if [ ${test5} == "true" ]
                              then
                                echo "Test5 passed"
                                exit 0
                              else
                                exit 42
                              fi'''
          }
          }
          post {
            always {
              echo 'I will always say Hello again! - From Test 5'
            }
            failure {
                script {
                    env.TEST5_RESULT = "false"
                    }
               sh '''echo "Test 5 failed! "
               echo ${TEST5_RESULT}'''
             }
            success {
              echo "Test 5 succeeded"
              exit 0
            }
          }
        }
      }
    }
    stage('deploy') {
    when { environment name: 'TEST5_RESULT', value: 'true' }
      steps {
      sh '''echo "Test 5 Result is: "
            echo ${TEST5_RESULT}
            echo "Test 3 Result is: "
            echo ${TEST3_RESULT}'''
            echo 'Deploying'
      }
    }
  }
  environment {
    test5 = 'true'
  }
}