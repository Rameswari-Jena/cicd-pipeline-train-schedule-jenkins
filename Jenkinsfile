pipeline{
    agent {label 'centos-node1'}
    checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'jkey-for-git', url: 'https://github.com/Rameswari-Jena/cicd-pipeline-train-schedule-jenkins']]])
    parameters ([
            choice(choices: ['ios', 'android'], description: 'Choose between two different platforms', name: 'Choose Platform')
        ])
    stages{
        stage('Test & Build'){
            steps{
                scripts {
                    def build() {
                        cleanWs()
                        nodejs('Node-16.9.1')
                        sh "yarn install"
                        for choice in ${params.Choose Platform}
                            sh "yarn test choice"
                            sh "yarn build choice"
                        }
                    }
                }
            }
        }
        stage('build'){
                steps {
                    cleanWs()  // Clean before build
                    checkout scm  // We need to explicitly checkout from SCM here
                    scripts {
                        def build() {
                            for choice in 
                        }
                    }
                }
            }
        }   
    }
    post {
        always {
            deleteDir() /* clean up workspace after build */
        }
    }
}
