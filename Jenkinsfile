pipeline{
	agent {label 'centos-node1'}
	
	tools {nodejs 'Node-10.24.1'}
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
                script {
                    if (params.platform == 'ios') {
                        sh "echo "testing on ios" > test_result.txt"
                    }
					else {
						sh "echo 'testing on android' > test_result.txt"
					}
                }
		}
		stage('Upload artifact to S3') {
			steps {
				s3Upload consoleLogLevel: 'INFO', dontSetBuildResultOnFailure: false, dontWaitForConcurrentBuildCompletion: false, entries: [[bucket: ' mobilebuild5', excludedFile: '', flatten: false, gzipFiles: true, keepForever: false, managedArtifacts: true, noUploadOnFailure: false, selectedRegion: 'us-east-1', showDirectlyInBrowser: false, sourceFile: '/home/jenkins/workspace/**/*', storageClass: 'STANDARD', uploadFromSlave: false, useServerSideEncryption: false, userMetadata: [[key: 'Name', value: 'built artifacts']]]], pluginFailureResultConstraint: 'FAILURE', profileName: 'S3-As-artifact storage', userMetadata: []
				}	
			}
		}
	}
}
