pipeline {
    agent any
    environment {
        SONARQUBE = 'Sonarqube'
    }
    stages {
        stage('Test SonarQube Scanner') {
            steps {
                script {
                    // Check if the tool is available
                    def scannerHome = tool name: 'SonarQubeScanner', type: 'SonarScanner'
                    echo "SonarQube Scanner found at: ${scannerHome}"
                }
            }
        }
        stage('SonarQube Analysis') {
            steps {
                script {
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
