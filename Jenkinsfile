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
            echo 'Job was successful!!'
        }
        failure {
            echo 'Job was unsuccessful!'
        }
    }
}
