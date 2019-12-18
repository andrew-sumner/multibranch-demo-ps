pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                bat label: 'Build artifact', script: 'echo "This is Jenkins build %BUILD_NUMBER% on branch %BRANCH_NAME%" > Build.txt'

				archiveArtifacts '*.txt'
				
				powershell label: 'Trigger BuildMaster Build', script: '''
				  Invoke-WebRequest http://inedo:8622/api/releases/builds/create `
				    -ContentType "application/json" `
				    -Method POST `
				    -Body \'{"applicationName": "hdars", "releaseNumber": "2.14.0", "$JenkinsBuildNumber": "88512" }\' `
				    -Headers @{"X-ApiKey" = "<api-key>"}
				'''
            }
        }
    }
} 
