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
            echo 'Hello World. Build Stage 2'
          }
        }
      }
    }
    stage('Push') {
      steps {
        echo 'Pushing Build'
        sleep 3
      }
    }
    stage('Test') {
      parallel {
        stage('Test2') {
          steps {
            echo "Running Test 2"
            sh '''echo "Running test 2. Value of test 2 env is:"
                  echo ${test2}
                  if [ ${test2} == "true" ]
                  then
                  echo "Test 2 passed"
                  else
                  exit 42
                  fi'''
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
                script {
                  env.TEST2_RESULT = "true"
                }
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
    test2 = 'false'
  }
}