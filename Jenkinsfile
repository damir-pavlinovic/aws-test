void setBuildStatus(String message, String state) {
  step([
      $class: "GitHubCommitStatusSetter",
      reposSource: [$class: "ManuallyEnteredRepositorySource", url: "https://github.com/damir-pavlinovic/aws-test.git"],
      contextSource: [$class: "ManuallyEnteredCommitContextSource", context: "test"],
      errorHandlers: [[$class: "ChangingBuildStatusErrorHandler", result: "UNSTABLE"]],
      statusResultSource: [ $class: "ConditionalStatusResultSource", results: [[$class: "AnyBuildResult", message: message, state: state]] ]
  ]);
}


pipeline {
  agent {label 'build'}
  stages {
    stage('Build') {
      steps {
        sh 'gcc *.c -o main.exe'
        sh './main.exe'
      }
    }
    stage('Upload') {
      steps {
	s3Upload consoleLogLevel: 'INFO', 
	  dontSetBuildResultOnFailure: false, 
	  dontWaitForConcurrentBuildCompletion: false, 
	  entries: [[bucket: "pavlinovic-test-bucket", 
	    excludedFile: '', flatten: false, gzipFiles: false, 
	    keepForever: false, managedArtifacts: false, noUploadOnFailure: true, 
	    selectedRegion: 'eu-central-1', showDirectlyInBrowser: false, sourceFile: '**/*.exe', 
            storageClass: 'STANDARD', uploadFromSlave: true, useServerSideEncryption: false]], 
	  pluginFailureResultConstraint: 'FAILURE', profileName: 'S3-Artifact', userMetadata: []
      }
    }
  }
  post {
    success {
      setBuildStatus("Build succeeded!", "SUCCESS");
    }
    failure {
      setBuildStatus("Build failed!", "FAILURE");
    }
  }
}
