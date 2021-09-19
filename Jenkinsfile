pipeline{
    agent {label 'centos-node1'}
	stages {
        stage('Setup parameters') {
            steps {
                script { 
                    properties([
                        parameters([
                            choice(
                                choices: ['ios', 'android'], 
                                name: 'Choose_platform'
                            )
						])
					])
				}
			}
		}
		stage('Build') {
            when {
                expression { 
                   return params.ENVIRONMENT == 'ios'
                }
            }
            steps {
                    sh "yarn test ios"
		    sh "yarn build ios"
                }
            }
   }
}
	post {
        always {
            deleteDir() /* clean up workspace after build */
        }
    }
