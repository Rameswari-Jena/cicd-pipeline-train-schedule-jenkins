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
        stage ('test') {
            when {
                // do yarn test ios when ios platform is selected
                expression { params.platform == 'ios' }
            }
            steps {
				script{
					sh "yarn test ios"
					//yarn build ios
					//echo "building on ios!"
				}
			}
		}
		stage('Upload artifact to S3') {
        dir('/home/jenkins/workspace/'){
            withAWS(region:'ua-east-1',credentials:'newly-created-credentials-ID') {
                 def identity=awsIdentity();
                // Upload files from working directory to project workspace
                s3Upload(bucket:"mobilebuild5", workingDir:'/home/jenkins/workspace/', includePathPattern:'**/*');
				}

			};
		}
	}
}
