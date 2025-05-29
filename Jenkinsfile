pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('sonar-token') // Jenkins Secret Text Credential
        SONAR_SCANNER_DIR = 'sonar-scanner-5.0.1.3006-linux'
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
                    if [ ! -d "$SONAR_SCANNER_DIR" ]; then
                        echo "📦 Downloading sonar-scanner..."
                        wget https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/${SONAR_SCANNER_DIR}.zip
                        unzip ${SONAR_SCANNER_DIR}.zip
                    fi
                    echo "🚀 Running sonar-scanner..."
                    ${SONAR_SCANNER_DIR}/bin/sonar-scanner \
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
