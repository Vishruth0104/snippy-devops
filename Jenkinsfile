pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('sonar-token')
        PYTHON_ENV = 'venv'
    }

    stages {
        stage('Build') {
            steps {
                sh '''
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install --upgrade pip
                    pip install -r backend/requirements.txt
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                    . venv/bin/activate
                    pip install pytest
                    pytest backend/tests/
                '''
            }
        }

        stage('Code Quality') {
            steps {
                sh '''
                    sonar-scanner \
                      -Dsonar.projectKey=Vishruth0104_snippy-devops \
                      -Dsonar.organization=vishruth0104 \
                      -Dsonar.sources=backend \
                      -Dsonar.host.url=https://sonarcloud.io \
                      -Dsonar.python.version=3.10 \
                      -Dsonar.login=$SONAR_TOKEN
                '''
            }
        }

        stage('Security Scan') {
            steps {
                echo '🔒 Running security scans (placeholder)'
                // Example: run safety or bandit here
            }
        }

        stage('Deploy') {
            steps {
                echo '🚀 Deploying application (placeholder)'
                // Add deploy commands here
            }
        }
    }

    post {
        success {
            echo '✅ Build completed successfully!'
        }
        failure {
            echo '❌ Build failed. Check logs for details.'
        }
    }
}
