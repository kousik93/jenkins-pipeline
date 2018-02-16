pipeline {
 agent any
 stages {

 //Performing 2 builds in parallel
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

  //Unconditionally pushing the build
  stage('Push') {
   steps {
    echo 'Pushing Build'
    sleep 3
   }
  }

  /*
  In this we have 2 tests running in parallel.
  Both tests will read an ENV variable and determine if they want to fail or not.
  To achieve AND and OR conditions in deploy step next, we need to:
  - Wrap our stage code in catchError{} (This will continue building pipeline even if the stage fails)
  - At last, we should set an ENV variable suggesting this stage passed
  */
  stage('Test') {
   parallel {
    stage('Test1') {
     steps {

      catchError {
       echo "Running Test 1"
       script {
        if (env.test1 == "false")
         error 'Test 1 failed!'
       }
       // If stage succeded, set ENV variable
       script {
        env.TEST1_RESULT = "true"
       }
      }
     }
     /*Check what var value is.
     This post block suffers from race condition issues when a part of parallel execution
     So cannot use failure{} and success{} blocks here
     */
     post {
      always {
       echo "Test 1 Result"
       sh ''
       'echo ${TEST1_RESULT}'
       ''
      }
     }
    }
    stage('Test2') {
     steps {
      catchError {
       echo "Running Test 2"
       script {
        if (env.test2 == "false")
         error 'Test 2 failed!'
       }
        script {
         env.TEST2_RESULT = "true"
        }
      }
     }
     post {
      always {
       echo "Test 2 Result"
       sh ''
       'echo ${TEST2_RESULT}'
       ''
      }
     }
    }
   }
  }
  // In the deploy stage, I only check Test 2 Result.
  // It will run as long as Test 2 passed and doesnt depend on Test 1
  stage('deploy') {
   when {
    environment name: 'TEST2_RESULT', value: 'true'
   }
   steps {
    sh ''
    'echo "Test 2 Result is: "
    echo $ {
     TEST2_RESULT
    }
    echo "Test 1 Result is: "
    echo $ {
     TEST1_RESULT
    }
    ''
    '
    echo 'Deploying'
   }
  }
 }
 environment {
  test2 = 'true'
  test1 = 'true'
 }
}