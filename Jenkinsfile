pipeline{
	agent {label 'centos-node1'}
	
	environment{
		PATH = "/usr/share/doc/:$PATH"
	}
	stages{
		stage('git checkout') {
			steps{
				git credentialsId: 'github-account', url: 'https://github.com/Rameswari-Jena/cicd-pipeline-train-schedule-jenkins'
			}
		}
		stage('Setup parameters') {
            steps {
                script { 
                    properties([
                        parameters([
                            choice(
                                choices: ['ios', 'android'], 
                                name: 'platform'
                            )
						])
					])
				}
			}
		}
        stage ('unit-test') {
            steps {
                
                if (${params.platform} =='ios') {
                    echo "executing yarn on ios"
					nodejs('Node-10.24.1'){
						sh "yarn test ios"
						sh "yarn build ios"
					}
                }
				else {
					echo "executing yarn on android"
					nodejs('Node-10.24.1'){
						sh "yarn test android"
						sh "yarn build android" 
					}
                }
			}
		}
		stage('Upload artifact to S3') {
			steps {
				s3Upload consoleLogLevel: 'INFO', dontSetBuildResultOnFailure: false, dontWaitForConcurrentBuildCompletion: false, entries: [[bucket: ' mobilebuild5', excludedFile: '', flatten: false, gzipFiles: true, keepForever: false, managedArtifacts: true, noUploadOnFailure: false, selectedRegion: 'us-east-1', showDirectlyInBrowser: false, sourceFile: '/home/jenkins/workspace/output/*', storageClass: 'STANDARD', uploadFromSlave: false, useServerSideEncryption: false, userMetadata: [[key: 'Name', value: 'built artifacts']]]], pluginFailureResultConstraint: 'FAILURE', profileName: 'S3-As-artifact storage', userMetadata: []
			}	
		}
	}
	post { 
        always { 
            cleanWs()
        }
    }
}
