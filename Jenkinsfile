pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
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
                pip install flake8 pytest bandit
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

        stage('Security Scan - SAST') {
            steps {
                sh '''
                . venv/bin/activate
                bandit -r . -x ./venv
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

        always {
            echo 'Pipeline execution completed.'
        }
    }
}
