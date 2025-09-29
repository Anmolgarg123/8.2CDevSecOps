pipeline {
    agent any

    environment {
        PROJECT_NAME = '8.2CDevSecOps'
        EMAIL_RECIPIENTS = "garganmol233@gmail.com"
        CONSOLE_LOG = 'console-log.txt'
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
            script {
                // Capture console log and save to file
                writeFile file: env.CONSOLE_LOG, text: currentBuild.rawBuild.getLog(2000).join("\r\n")
            }

            // Send email with console log attached
            emailext(
                subject: "${PROJECT_NAME} - Build #${BUILD_NUMBER} - ${currentBuild.currentResult}",
                body: """Hello,

The Jenkins pipeline for project '${PROJECT_NAME}' has completed.

Build Number: ${BUILD_NUMBER}
Status: ${currentBuild.currentResult}

Please find the attached console log for details.""",
                to: env.EMAIL_RECIPIENTS,
                attachmentsPattern: env.CONSOLE_LOG
            )
        }
    }
}
