pipeline{
	agent {label 'centos-node1'}
	parameters {
        choice(
            choices: ['ios' , 'android'],
            description: 'choose platform',
            name: 'platform')
    }
	stages{
		stage('git checkout') {
			steps{
				git credentialsId: 'github-account', url: 'https://github.com/Rameswari-Jena/cicd-pipeline-train-schedule-jenkins'
			}
		}
		
		stages {
        stage ('test') {
            when {
                // do yarn test ios when ios platform is selected
                expression { params.platform == 'ios' }
            }
            steps {
				sh """
					yarn test ios
					yarn build ios
					echo "building on ios!"
					"""
				}
			}
		}
	}
}
