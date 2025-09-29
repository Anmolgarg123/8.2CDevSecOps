pipeline {
    agent any

    environment {
        PROJECT_NAME = '8.2CDevSecOps'
        EMAIL_RECIPIENTS = "garganmol233@gmail.com"
        CONSOLE_LOG = "${WORKSPACE}\\console-log.txt"
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
            // Collect all .log files into console-log.txt
            bat """
                @echo off
                if exist "${WORKSPACE}\\console-log.txt" del "${WORKSPACE}\\console-log.txt"
                for /r "%WORKSPACE%" %%f in (*.log) do type "%%f" >> "${WORKSPACE}\\console-log.txt"
            """

            // Check if the file exists
            script {
                if (fileExists(env.CONSOLE_LOG)) {
                    echo "Console log file exists: ${env.CONSOLE_LOG}"
                } else {
                    echo "Console log file does NOT exist!"
                }
            }

            // Send email with the console log attached (if exists)
            emailext(
                subject: "${PROJECT_NAME} - Build #${BUILD_NUMBER} - ${currentBuild.currentResult}",
                body: "Pipeline finished. Console log attached if available.",
                to: "${EMAIL_RECIPIENTS}",
                attachmentsPattern: fileExists(env.CONSOLE_LOG) ? 'console-log.txt' : ''
            )
        }
    }
}
