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
        stage('Build 2 Parallel') {
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
        stage('Test1') {
          steps {
            script{
                env.TEST1_RESULT = "false"
            }
            catchError{
                echo "Running Test 1"

                script{
                    if (env.test1 == "false")
                    error 'Test 1 failed!'
                }
                script{
                    env.TEST1_RESULT = "true"
                }
            }
          }
          post {
            always {
                echo "test1 status"
                sh '''echo ${TEST1_RESULT}'''
            }
          }
        }
        stage('Test2') {
          steps {
            echo "Running Test 2. Setting to fail."
            script{
                env.Test2_RESULT = "false"
            }
            sh '''echo "Running test 2. Value of test 2 env is:"
                  echo ${test2}
                  if [ ${test2} == "true" ]
                  then
                  echo "Test 2 passed"
                  else
                  echo "EXITING"
                  exit 42
                  fi'''
            script{
                env.TEST2_RESULT = "true"
            }
          }
          post {
            always {
                echo "test2 status"
                sh '''echo ${TEST2_RESULT}'''
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
            echo "Test 1 Result is: "
            echo ${TEST1_RESULT}'''
            echo 'Deploying'
      }
    }
  }
  environment {
    test2 = 'true'
    test1 = 'true'
  }
}