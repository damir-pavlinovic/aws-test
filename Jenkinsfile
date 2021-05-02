pipeline {
  agent none
  stages {
    stage('Build') {
      agent {label 'build'}
      steps {
	      echo 'Hello'
      }
      post {
        success {
          step([
           $class: "GitHubCommitStatusSetter",
             reposSource: [$class: "ManuallyEnteredRepositorySource", url: "https://github.com/damir-pavlinovic/aws-test.git"],
             contextSource: [$class: "ManuallyEnteredCommitContextSource", context: "ci/jenkins/build-status"],
             errorHandlers: [[$class: "ChangingBuildStatusErrorHandler", result: "UNSTABLE"]],
             statusResultSource: [ $class: "ConditionalStatusResultSource", results: [[$class: "AnyBuildResult", message: "Build succeeded!", state: "SUCCESS"]]]
          ])
        }
	failure {
          step([
           $class: "GitHubCommitStatusSetter",
             reposSource: [$class: "ManuallyEnteredRepositorySource", url: "https://github.com/damir-pavlinovic/aws-test.git"],
             contextSource: [$class: "ManuallyEnteredCommitContextSource", context: "ci/jenkins/build-status"],
             errorHandlers: [[$class: "ChangingBuildStatusErrorHandler", result: "UNSTABLE"]],
             statusResultSource: [ $class: "ConditionalStatusResultSource", results: [[$class: "AnyBuildResult", message: "Build failed!", state: "FAILURE"]]]
          ])
        }
      }
    }
}
