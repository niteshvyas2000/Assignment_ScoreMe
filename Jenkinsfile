pipeline {
    agent any
    environment {
        SONARQUBE = 'Sonarqube'  // Name of the SonarQube instance configured in Jenkins
    }
    tools {
        sonarScanner 'SonarQubeScanner'
    }

    stages {
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv("${SONARQUBE}") { // Use the environment variable
                    sh 'sonar-scanner -Dsonar.projectKey=test-project -Dsonar.sources=src' 
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
