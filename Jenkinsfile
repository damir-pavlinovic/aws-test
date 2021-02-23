pipeline {
  agent {label 'build'}
  stages {
    stage('Build') {
      steps {
        sh 'aws s3 ls'
        echo 'With S3 policy'
        // sh 'gcc *.c -o main.exe'
        // sh './main.exe'
      }
    }
  }
  post {
        success {
            echo 'Job was successful!'
        }
        failure {
            echo 'Job was unsuccessful!'
        }
    }
}
