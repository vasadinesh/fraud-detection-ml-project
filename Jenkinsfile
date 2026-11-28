
pipeline {
    agent {
        label 'predict'
    }

    environment {
        APP_NAME = 'fraud-detection-ml-project'
        PYTHONPATH = "${WORKSPACE}"
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
                sh '''
                    python3 --version
                    python3 -m pip --version
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                    set -e

                    python3 -m venv venv
                    . venv/bin/activate

                    python -m pip install --upgrade pip
                    pip install -r requirements.txt

                    # Install backend-specific dependencies if required
                    if [ -f backend/requirements.txt ]; then
                        pip install -r backend/requirements.txt
                    fi
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                    set -e

                    . venv/bin/activate

                    export PYTHONPATH="$WORKSPACE:$PYTHONPATH"

                    echo "Checking backend import..."

                    python -c "import backend; print('Backend imported successfully')"

                    echo "Running unit tests..."

                    if [ -d tests ]; then
                        python -m pytest tests/ -v
                    else
                        echo "No tests directory found. Skipping tests."
                    fi
                '''
            }
        }

        stage('Build Check') {
    steps {
        sh '''
            set -e

            . venv/bin/activate

            echo "Checking Python syntax..."

            python - <<'PY'
import ast
from pathlib import Path

for path in Path("backend").rglob("*.py"):
    ast.parse(path.read_text(encoding="utf-8"), filename=str(path))

print("Python build check successful")
PY
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
