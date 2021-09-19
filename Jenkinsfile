pipeline{
    agent {label 'centos-node1'}
    checkout([$class: 'GitSCM', branches: [[name: 'master']],userRemoteConfigs: [[credentialsId: 'jkey-for-git', url: 'https://github.com/Rameswari-Jena/cicd-pipeline-train-schedule-jenkins']]])
    parameters ([
            choice(choices: ['ios', 'android'], description: 'Choose between two different platforms', name: 'Choose Platform')
        ])
    stages{
        stage('Test & Build'){
            steps{
                scripts {
                    cleanWs()
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
