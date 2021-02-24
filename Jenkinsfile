pipeline {
  agent none
  stages {
    stage('Build') {
      agent {label 'build'}
      steps {
        sh 'gcc *.c -o main.exe'
        sh './main.exe'
	sh 'ls'
      }
    }
    stage('Upload') {
      environment {
        FULL_PATH_BRANCH = "${sh(script:'git name-rev --name-only HEAD', returnStdout: true)}"
        BRANCH = FULL_PATH_BRANCH.substring(FULL_PATH_BRANCH.lastIndexOf('/') + 1, FULL_PATH_BRANCH.length())
        STORAGE_LOCATION = "pavlinovic-test-bucket/" + "${env.BRANCH}"
      }
      agent {label 'build'}
      steps {
	s3Upload consoleLogLevel: 'INFO', 
	  dontSetBuildResultOnFailure: false, 
	  dontWaitForConcurrentBuildCompletion: false, 
	  entries: [[bucket: "${env.STORAGE_LOCATION}", 
	    excludedFile: '', flatten: false, gzipFiles: false, 
	    keepForever: false, managedArtifacts: false, noUploadOnFailure: true, 
	    selectedRegion: 'eu-central-1', showDirectlyInBrowser: false, sourceFile: '**/*.exe', 
            storageClass: 'STANDARD', uploadFromSlave: true, useServerSideEncryption: false]], 
	  pluginFailureResultConstraint: 'FAILURE', profileName: 'S3-Artifact', userMetadata: []
      }
    }
  }
}
