pipeline {
    agent {
        docker {
            image 'python:3.11-slim'
        }
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Setup Environment') {
            steps {
                sh '''
                    python -m venv venv
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
                    python -m compileall .
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
