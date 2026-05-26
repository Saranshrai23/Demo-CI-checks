pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                echo 'Code checkout completed'
            }
        }

        stage('Setup Python Environment') {
            steps {
                sh '''
                python3 --version
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
                python -m py_compile app.py
                '''
            }
        }

        stage('Lint Check') {
            steps {
                sh '''
                . venv/bin/activate
                flake8 app.py test_app.py
                '''
            }
        }

        stage('Unit Test') {
            steps {
                sh '''
                . venv/bin/activate
                pytest
                '''
            }
        }
    }

    post {
        success {
            echo 'CI checks passed successfully.'
        }
        failure {
            echo 'CI checks failed. Please fix the issue.'
        }
    }
}
