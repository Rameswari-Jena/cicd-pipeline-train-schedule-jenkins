pipeline{
	agent {label 'centos-node1'}
	
	tools {nodejs 'Node-10.24.1'}
	environment{
		PATH = "give-full-path-of-the-tool:$PATH"
	}
	stages{
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
		stage('clean workspace'){
			steps {
				cleanWs()
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
        stage ('unit-test') {
            steps {
				script{
					echo "current build number: ${currentBuild.number}"
					echo "Into Script"
					echo params.platform
					if (params.platform =='ios') {
						echo "executing yarn on ios"
						sh "yarn test "	
					}
					else if (params.platform =='android') {
						echo "executing yarn on android"
						sh "yarn test "
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
		stage('Upload artifact to S3') {
			steps {
				script {
					if (params.platform =='ios') {
						script {
							dir('/home/jenkins/workspace/AD'){

								//configure to aws profile
								sh "aws configure set aws_access_key_id AKIA52GGWPL2K26AT6XA" 
								sh "aws configure set aws_secret_access_key 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"
								sh "aws configure set region us-east-1"
								sh "aws s3 ls"
								// Upload artifact from project workspace to aws s3 bucket
								sh "aws s3 cp ios.txt s3://mobilebuild5/"
							}
						}
					}	
					else if (params.platform =='android') {
						script {
							dir('/home/jenkins/workspace/AD') {
								//configure to aws profile
								sh "aws configure set aws_access_key_id AKIA52GGWPL2K26AT6XA" 
								sh "aws configure set aws_secret_access_key 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"
								sh "aws configure set region us-east-1"
								sh "aws s3 ls"
								// Upload files from working directory to project workspace
								sh "aws s3 cp android.txt s3://mobilebuild5/"
							}
						}
					}
				}
			}
		}
		
	}
}
