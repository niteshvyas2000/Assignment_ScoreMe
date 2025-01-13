pipeline {
    agent any
    environmnt {
        SONARQUBE = 'Sonarqube'  // Name of the SonarQube instance configured in Jenkins
    }
    tools {
        sonarQube 'SonarQubeScanner' // Replace with your SonarQube Scanner name
    }

    stages {
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('Sonarqube') { // Replace with your SonarQube server name
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
