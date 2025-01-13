pipeline {
    agent any
    environment {
        SONARQUBE = 'Sonarqube'  // Name of the SonarQube instance configured in Jenkins
    }
    stages {
        stage('SonarQube Analysis') {
            steps {
                script {
                    // Ensure the SonarQube Scanner is installed and available
                    def scannerHome = tool name: 'SonarQubeScanner', type: 'SonarScanner' 
                    withSonarQubeEnv("${SONARQUBE}") { 
                        sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=test-project -Dsonar.sources=src"
                    }
                }
            }
        }
        stage('Quality Gate') {
            steps {
                script {
                    def qg = waitForQualityGate()
                    if (qg.status != 'OK') {
                        error "Pipeline aborted due to quality gate failure: ${qg.status}"
                    }
                }
            }
        }
    }
}
