pipeline {
  agent {label 'build'}
  stages {
    stage('Build') {
      steps {
        sh 'pwd'
        sh 'ls'
        sh 'gcc --version'
        sh 'git branch'
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
