pipeline {
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

        stage('Install Dependencies') {
            steps {
                sh '''
                    npm install
                '''
            }
        }
        
        stage('Build image') {
            steps {
                sh '''
                   docker build -t catalogue:${appversion} .
                   docker images
                '''
            }
        }

        stage('Deploy') {
            when {
                expression { params.DEPLOY == true }
            }
            steps {
                sh '''
                    echo "Deploying application..."
                '''
            }
        }
    }

    post {
        always {
            echo 'Post step: always'
            cleanWs()
        }
        success {
            echo 'Post step: success'
        }
        failure {
            echo 'Post step: failure'
        }
        aborted {
            echo 'Post step: aborted'
        }
    }
}
