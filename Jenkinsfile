pipeline {
  agent {label 'build'}
  stages {
    stage('Build') {
      steps {
        echo 'With S3 policy, again'
        sh 'aws s3 ls'
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
