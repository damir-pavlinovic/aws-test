pipeline {
  environment {
    FULL_PATH_BRANCH = "${sh(script:'git name-rev --name-only HEAD', returnStdout: true)}"
    BRANCH = FULL_PATH_BRANCH.substring(FULL_PATH_BRANCH.lastIndexOf('/') + 1, FULL_PATH_BRANCH.length())
    STORAGE_LOCATION = "pavlinovic-test-bucket/" + "${env.BRANCH}"
  }
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
	  entries: [[bucket: "${env.STORAGE_LOCATION}", 
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
      withCredentials([usernamePassword(credentialsId: '8cd728c4-d8d4-4504-8fa0-dcca9c29e91d', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
        sh 'curl -X POST --user $USERNAME:$PASSWORD --data  "{\\"state\\": \\"success\\"}" --url $GITHUB_API_URL/statuses/$GIT_COMMIT'
      }
    }
    failure {
      withCredentials([usernamePassword(credentialsId: '8cd728c4-d8d4-4504-8fa0-dcca9c29e91d', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
        sh 'curl -X POST --user $USERNAME:$PASSWORD --data  "{\\"state\\": \\"failure\\"}" --url $GITHUB_API_URL/statuses/$GIT_COMMIT'
      }
    }
  }
}
