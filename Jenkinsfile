pipeline {
    agent any
    triggers {
        pollSCM('H/2 * * * *')
    }
    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'installing python dependencies'

                sh '''
                    python3 -m venv .venv
                    .venv/bin/pip install -r requirements.txt
                '''

            }
        }
        stage('Test') {
            steps {
                echo 'Running Automated test'

                sh '''
                    .venv/bin/python -m pytest
                '''
                
            }
        }
        stage('Code Quality') {
            steps {

        withCredentials([
            string(
                credentialsId: 'sonar-token',
                variable: 'SONAR_TOKEN'
            )
        ]) {

            sh '''
                export PATH=/opt/sonar-scanner/bin:$PATH
                sonar-scanner \
                -Dsonar.host.url=http://localhost:9000 \
                -Dsonar.token=$SONAR_TOKEN
            '''
        }
    }

        }
    }
}