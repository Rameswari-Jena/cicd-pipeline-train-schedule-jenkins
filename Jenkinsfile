pipeline{
	agent {label 'centos-node1'}
	properties([
		parameters([choice(choices: ['ios', 'android'], description: 'choose-platform', name: 'platform')])
		])
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
		
		stage('clean workspace'){
			steps {
				cleanWs()
			}
		}
        stage ('unit-test') {
            steps {
				script{
					if (param.platform =='ios') {
						echo "executing yarn on ios"
						nodejs('Node-10.24.1'){
							sh "yarn test "
							sh "yarn build "
						}
					}
					else {
						echo "executing yarn on android"
						nodejs('Node-10.24.1'){
							sh "yarn test "
							sh "yarn build"
						}
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
	//post { 
        //always { 
            //cleanWs()
        //}
    //}
}
