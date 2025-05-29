pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('sonar-token') // 👈 Add this in Jenkins > Manage Credentials
        PATH = "/opt/homebrew/bin:$PATH"         // 👈 Ensure sonar-scanner from Homebrew is accessible
    }

    stages {
        stage('Build') {
            steps {
                echo "📦 Creating virtual environment and installing dependencies..."
                sh '''
                    python3 -m venv venv
                    . venv/bin/activate && pip install --upgrade pip
                    . venv/bin/activate && pip install -r backend/requirements.txt
                '''
            }
        }

        stage('Test') {
            steps {
                echo "🧪 Running tests with pytest..."
                sh '''
                    . venv/bin/activate
                    pip install pytest
                    pytest backend/tests/
                '''
            }
        }

        stage('Code Quality') {
            steps {
                echo "🔍 Running SonarCloud analysis..."
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

        // Optional: Add more stages like 'Security Scan' or 'Deploy'
    }

    post {
        success {
            echo '✅ Pipeline completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed. Check the logs above.'
        }
    }
}
