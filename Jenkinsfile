
pipeline {
    agent any

    environment {
        APP_NAME = 'fraud-detection-ml-project'
    }

    stages {

        stage('Git Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/vasadinesh/fraud-detection-ml-project.git'
            }
        }

        stage('Python Version') {
            steps {
                sh 'python3 --version'
                sh 'pip3 --version'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install --upgrade pip
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                    . venv/bin/activate
                    if [ -d tests ]; then
                        pytest tests/ -v
                    else
                        echo "No tests directory found. Skipping tests."
                    fi
                '''
            }
        }

        stage('Build Check') {
            steps {
                sh '''
                    . venv/bin/activate
                    python3 -m compileall -q .
                    echo "Python build check successful"
                '''
            }
        }
    }

    post {
        success {
            echo 'Fraud Detection ML Pipeline SUCCESS!'
        }
        failure {
            echo 'Fraud Detection ML Pipeline FAILED!'
        }
    }
}
