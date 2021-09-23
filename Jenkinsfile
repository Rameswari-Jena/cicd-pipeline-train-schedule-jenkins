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
			post{
				success {
					echo "checkout master branch sucessful"
				}
				failure {
					script{
						sh "exit 1"
					}
                }
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
		stage('Upload artifact to S3') {
			steps {
				
				
				script {
					//configure to aws profile
					sh "aws configure set aws_access_key_id AKIA52GGWPL2K26AT6XA aws_secret_access_key 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 region us-east-1"
					sh "aws s3 ls"
					if (params.platform =='ios') {
						dir('/home/jenkins/workspace/AD'){
							withAWS(region:'us-east-1',credentials:'AWS Credential',role:'Pipeline-Project', roleAccount: '949604481780', useNode:true) {
								def identity=awsIdentity();
								echo "hi aws user"
								// Upload artifact from project workspace to aws s3 bucket
								aws s3 cp /home/jenkins/workspace/AD/ios.txt s3://mobilebuild5/ --recursive --exclude "*" --include "*.txt"
							}
						};
						
					}	
					else if (params.platform =='android') {
						withAWS(region:'us-east-1',credentials:'AWS Credential') {
							def identity=awsIdentity();
							// Upload files from working directory to project workspace
							s3Upload(bucket:"mobilebuild5", workingDir:'/home/jenkins/workspace/AD/android.txt', includePathPattern:'**/android.txt');
						}
						
					}
				}
			}
		}
		stage('clean workspace'){
			steps {
				//cleanWs()
				echo "clean up"
			}
			post{
				success {
					echo "clean up is done"
				}
				failure {
					script{
						sh "exit 1"
					}
                }
            }
		}
		
		
		
        stage ('unit-test') {
            steps {
				script{
					echo "current build number: ${currentBuild.number}"
					echo "Into Script"
					echo params.platform
					if (params.platform =='ios') {
						echo "executing yarn on ios"
						sh "yarn test ios"	
					}
					else if (params.platform =='android') {
						echo "executing yarn on android"
						sh "yarn test android"
					}
				}
				post{
					success {
						echo "test is successful"
					}
					failure {
						script{
							sh "exit 1"
						}
                    }
                }
			}
		}
		stage ('Build') {
            steps {
				script{
					echo "Building started"
					echo params.platform
					if (params.platform =='ios') {
						echo "executing yarn on ios"
						sh "yarn build ios"
						
					}
					else if (params.platform =='android'){
						echo "executing yarn on android"
						sh "yarn build android"
					}
				}
				post{
					success {
						echo "build is successful"
					}
					failure {
						script{
							sh "exit 1"
						}
                    }
                }
			}
		}
		
	}
}
