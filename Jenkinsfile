pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Setup Python Environment') {
            steps {
                sh '''
                python3 -m venv venv
                . venv/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

        stage('Syntax Check') {
            steps {
                sh '''
                . venv/bin/activate
                python3 -m compileall .
                '''
            }
        }

        stage('Unit Test') {
            steps {
                sh '''
                . venv/bin/activate
                mkdir -p test-reports
                pytest test_app.py --junitxml=test-reports/results.xml
                '''
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }
    }

    post {
        success {
            echo 'Python CI Pipeline completed successfully!'
        }
        failure {
            echo 'Python CI Pipeline failed!'
        }
    }
}
