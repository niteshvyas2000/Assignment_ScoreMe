pipeline {
    agent any

    environment {
        SONARQUBE = 'Sonarqube'  // Name of the SonarQube instance configured in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        // stage('Build') {
        //     steps {
        //         // Build your project here (e.g., Maven, Gradle, npm)
        //         sh 'mvn clean install' // Example for Maven
        //     }
        // }

        stage('SonarQube Analysis') {
            steps {
                script {
                    // Run SonarQube analysis
                    sonarScanner(
                        // Use the configured SonarQube server from Jenkins
                        installation: "${env.SONARQUBE}",
                        properties: [
                            "sonar.projectKey=test-project",  // Your project key in SonarQube
                            "sonar.projectName=test-project",     // Your project name in SonarQube
                            "sonar.sources=src"                // The source directory for the analysis
                        ]
                    )
                }
            }
        }

        stage('Quality Gate') {
            steps {
                // Wait for the analysis results and quality gate status
                waitForQualityGate abortPipeline: true
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
