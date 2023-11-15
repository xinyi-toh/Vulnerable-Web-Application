pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/xinyi-toh/Vulnerable-Web-Application.git'
            }
        }
        stage('Code Quality Check via SonarQube') {
            // environment {
            //     SONAR_PROJECT_KEY = 'OWASP'
            //     SONAR_SOURCES = '.'
            // }
            steps {
                script {
                    def scannerHome = tool 'SonarQube'
                    withSonarQubeEnv('SonarQube') {
                        def scannerCmd = "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=${SONAR_PROJECT_KEY} -Dsonar.sources=${SONAR_SOURCES}"
                        echo "SonarQube Scanner Command: ${scannerCmd}"
                        def scannerStatus = sh script: scannerCmd, returnStatus: true
                        if (scannerStatus != 0) {
                            error "SonarQube scanner failed with exit code: ${scannerStatus}"
                        }
                    }
                }
            }
        }
    }
    post {
        always {
            recordIssues enabledForFailure: true, tool: sonarQube()
        }
    }
}
