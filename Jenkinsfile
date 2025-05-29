pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('sonar-token') // Make sure this credential is created in Jenkins
    }

    stages {
        stage('Build') {
            steps {
                sh '''
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install --upgrade pip
                    pip install -r backend/requirements.txt
                    which pytest
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
                    export PATH=$PATH:$(pwd)/sonar-scanner-5.0.1.3006-linux/bin
                    sonar-scanner \
                        -Dsonar.projectKey=Vishruth0104_snippy-devops \
                        -Dsonar.organization=vishruth0104 \
                        -Dsonar.sources=backend \
                        -Dsonar.host.url=https://sonarcloud.io \
                        -Dsonar.login=$SONAR_TOKEN
                '''
            }
        }
    }
}
