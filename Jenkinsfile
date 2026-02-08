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

        stage('Install Dependencies') {
            steps {
                sh '''
                    pip install --upgrade pip
                    pip install pytest
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                    pytest test_basic.py
                '''
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
