pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('sonar-token') // Jenkins Secret Text Credential
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
                    pytest backend/tests/
                '''
            }
        }

        stage('Code Quality') {
            steps {
                sh '''
                    wget https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.0.1.3006-linux.zip
                    unzip sonar-scanner-cli-5.0.1.3006-linux.zip
                    sonar-scanner-5.0.1.3006-linux/bin/sonar-scanner \
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
                sh '''
                    . venv/bin/activate
                    pip install bandit
                    bandit -r backend
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo '✅ Deployment to staging simulated!'
            }
        }
    }

    post {
        success {
            echo '🎉 Build and analysis completed successfully!'
        }
        failure {
            echo '❌ Build failed. Check logs for details.'
        }
    }
}
