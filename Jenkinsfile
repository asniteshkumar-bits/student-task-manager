pipeline {
    agent any
    triggers {
        pollSCM('* * * * *')
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
    }
}