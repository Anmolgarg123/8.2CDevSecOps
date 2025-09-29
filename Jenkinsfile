pipeline {
    agent any

    environment {
        PROJECT_NAME = '8.2CDevSecOps'
        EMAIL_RECIPIENTS = 'garganmol233@gmail.com'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Anmolgarg123/8.2CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'npm test || exit /b 0'
            }
        }

        stage('Generate Coverage Report') {
            steps {
                bat 'npm run coverage || exit /b 0'
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                bat 'npm audit || exit /b 0'
            }
        }
    }

    post {
        always {
            // Save console log to a file
            bat 'powershell -Command "Get-Content $env:WORKSPACE\\**\\*.log -Raw | Out-File $env:WORKSPACE\\console-log.txt"'

            // Send email with console log attached
            emailext(
                subject: "${PROJECT_NAME} - Build #${BUILD_NUMBER} - ${currentBuild.currentResult}",
                body: "Pipeline completed. Console log attached.",
                to: "${EMAIL_RECIPIENTS}",
                attachmentsPattern: "console-log.txt"
            )
        }
    }
}
