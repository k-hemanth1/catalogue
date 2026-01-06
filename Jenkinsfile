ppipeline {
    agent {
        node {
            label 'AGENT-1'
        }
    }

    parameters {
        booleanParam(
            name: 'DEPLOY',
            defaultValue: false,
            description: 'Deploy the application'
        )
    }

    environment {
        COURSE = "Jenkins"
    }

    options {
        timeout(time: 10, unit: 'MINUTES')
        disableConcurrentBuilds()
    }

    stages {
        stage('Build') {
            steps {
                script {
                    def packageJSON = readJSON file: 'package.json'
                    def appversion = packageJSON.version
                    echo "App version: ${appversion}"
                }
            }
        }

        stage('Test') {
            steps {
                sh '''
                    echo "Testing application"
                '''
            }
        }

        stage('Deploy') {
            when {
                expression { params.DEPLOY == true }
            }
            steps {
                sh '''
                    echo "Deploying application"
                '''
            }
        }
    }

    post {
        always {
            echo 'I will always say Hello again!'
            cleanWs()
        }
        success {
            echo 'I will run if success'
        }
        failure {
            echo 'I will run if failure'
        }
        aborted {
            echo 'pipeline is aborted'
        }
    }
}
