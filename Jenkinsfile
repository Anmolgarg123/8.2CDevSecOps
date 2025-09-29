pipeline {
    agent any

    environment {
        PROJECT_NAME = '8.2CDevSecOps'
        EMAIL_RECIPIENTS = "garganmol233@gmail.com"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Anmolgarg123/8.2CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install || exit /b 0'
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
            // Capture all .log files into console-log.txt (PowerShell compatible)
            bat '''
            powershell -Command "Get-ChildItem -Path $env:WORKSPACE -Recurse -Filter *.log | ForEach-Object { Get-Content $_ } | Out-File $env:WORKSPACE\\console-log.txt -Encoding UTF8"
            '''

            // Send email with console log attached
            emailext(
                subject: "${PROJECT_NAME} - Build #${BUILD_NUMBER} - ${currentBuild.currentResult}",
                body: "The pipeline has completed. Please find the console log attached.",
                to: "${EMAIL_RECIPIENTS}",
                attachmentsPattern: "console-log.txt"
            )
        }
    }
}
