pipeline {
    agent {
        node {
            label 'AGENT-1'
        }
    }

    environment {
        COURSE    = "Jenkins"
        ACC_ID    = "445567085619"
        PROJECT   = "roboshop"
        COMPONENT = "catalogue"
        REGION    = "us-east-1"
    }

    options {
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds()
    }

    parameters {
        string(name: 'appVersion', description: 'Which app version you want to deploy')
        choice(name: 'deploy_to', choices: ['dev', 'qa', 'prod'], description: 'Pick environment')
    }

    stages {
        stage('Deploy') {
            steps {
                script {
                    withAWS(region: REGION, credentials: 'aws-creds') {
                        sh """
                        aws eks update-kubeconfig \
                          --region ${REGION} \
                          --name ${PROJECT}-${params.deploy_to}
                        """
                    }
                }
            }
        }
    }   
    post {
        always {
            echo 'I will always say Hello Again!'
            cleanWs()
        }
        success {
            echo 'I will run if pipeline success'
        }
        failure {
            echo 'I will run if pipeline failure'
        }
        aborted {
            echo 'Pipeline is aborted'
        }
    }
}
