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
                                name: 'platform'
                            )
						])
					])
				}
			}
		}
		stage('Build') {
            if  ${params.platform} == 'ios' {
				steps {
					script{
							sh "yarn test ios"
							sh "yarn build ios"
							echo "building source code for ios platform"
						}
					}
				}
			else {
				steps {
					script {
							sh "yarn test android"
							sh "yarn build android"
							echo "building source code for android platform"
						}
					}
				}   
			}

	post {
        always {
            deleteDir() /* clean up workspace after build */
        }
    }
