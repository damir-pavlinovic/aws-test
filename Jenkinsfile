pipeline {
  agent {label 'wrong_label'}
  stages {
    stage('Build') {
      steps {
        echo 'No master executor!'
      }
    }
  }
}
