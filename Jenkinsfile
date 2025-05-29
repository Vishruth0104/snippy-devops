stage('Code Quality') {
    steps {
        sh '''
            pip install -U pip setuptools wheel
            curl -sSLo sonar-scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-4.8.0.2856.zip
            unzip sonar-scanner.zip
            sonar-scanner-*/bin/sonar-scanner
        '''
    }
}

