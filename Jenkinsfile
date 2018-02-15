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
            script{
                if (env.test2 == 'true'){
                    exit 0;
                } else {
                    exit 40;
                }
            }
          }
          post {
            failure {
                echo "test2 failed"
                script {
                  env.TEST2_RESULT = "false"
                }
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
      }
    }
    stage('deploy') {
    when { environment name: 'TEST2_RESULT', value: 'true' }
      steps {
      sh '''echo "Test 2 Result is: "
            echo ${TEST2_RESULT}
            echo "Test 3 Result is: "
            echo ${TEST3_RESULT}'''
            echo 'Deploying'
      }
    }
  }
  environment {
    test5 = 'true'
    test2 = 'true'
  }
}