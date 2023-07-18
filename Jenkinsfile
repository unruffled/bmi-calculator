pipeline {
    agent any

    stages {
        stage('SCA - SonarQube Analysis') {
            steps {
                script {
                    // SonarQube analysis configuration
                    def scannerHome = tool 'SonarQubeScanner'
                    withSonarQubeEnv(installationName:'sonarqube-9.9.1',credentialsID:'jenkins-sonar') {
                        sudo sh "${scannerHome}/bin/sonar-scanner -X"
                    }
                }
            }
        }
        stage('Build') {
            steps {
                script {
                    // Build the Node.js and React app using npm
                    sh 'npm install' // Install dependencies
                    sh 'npm run build' // Build the app
                }
            }
        }
    }

    post {
        always {
            // Wait for SonarQube quality gate result and change pipeline status accordingly
            script {
                def qg = waitForQualityGate()
                if (qg.status != 'OK') {
                    error "Pipeline failed due to Quality Gate failure: ${qg.status}"
                }
            }
        }
    }
}
