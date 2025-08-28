pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        checkout scm
        sh 'echo "Checked out $BRANCH_NAME"'
      }
    }

    stage('CI on PR') {
      when { changeRequest() }   
      steps {
        sh '''
          echo " Running tests for PR..."
          echo "pretend-tests passed"
        '''
      }
    }

    stage('CD on master (placeholder)') {
      when { branch 'master' }  
      steps {
        sh '''
          echo "Deploy placeholder on master (will implement next)"
        '''
      }
    }
  }

  post {
    always {
      echo "Pipeline finished for branch: ${env.BRANCH_NAME}"
    }
  }
}
