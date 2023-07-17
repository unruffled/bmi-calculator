pipeline {
    agent any

    stages {
        stage('SCA - SonarQube Analysis') {
            steps {
                script {
                    // SonarQube analysis configuration
                    def scannerHome = tool 'SonarQubeScanner'
                    withSonarQubeEnv('SonarQube Server') {
                        sh "${scannerHome}/bin/sonar-scanner"
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
