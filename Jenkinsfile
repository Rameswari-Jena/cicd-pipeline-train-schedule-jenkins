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
	}
}
