pipeline {
    agent any

    environment {
        PYTHON_ENV = 'venv'
        SONAR_SCANNER = '/opt/homebrew/bin/sonar-scanner' // Set correct sonar-scanner path
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'python3 -m venv venv'
                sh '. venv/bin/activate && pip install --upgrade pip && pip install -r backend/requirements.txt'
            }
        }

        stage('Test') {
            steps {
                sh '. venv/bin/activate && pip install pytest && pytest backend/tests/'
            }
        }

        stage('Code Quality') {
            steps {
                withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
                    sh '''${SONAR_SCANNER} \
                      -Dsonar.projectKey=Vishruth0104_snippy-devops \
                      -Dsonar.organization=vishruth0104 \
                      -Dsonar.sources=backend \
                      -Dsonar.host.url=https://sonarcloud.io \
                      -Dsonar.python.version=3.9 \
                      -Dsonar.login=$SONAR_TOKEN'''
                }
            }
        }

        stage('Security Scan') {
            steps {
                echo '🔒 Run security scans here (e.g., Bandit, safety)'
            }
        }

        stage('Deploy') {
            steps {
                echo '🚀 Deployment stage (if applicable)'
            }
        }
    }

    post {
        success {
            echo '✅ Build, test, and scan successful!'
        }
        failure {
            echo '❌ Build failed. Check logs for details.'
        }
    }
}
