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
                // Capture output to log file
                bat "npm install | tee ${env.CONSOLE_LOG}"
            }
        }

        stage('Run Tests') {
            steps {
                bat "npm test || exit /b 0 | tee -a ${env.CONSOLE_LOG}"
            }
        }

        stage('Generate Coverage Report') {
            steps {
                bat "npm run coverage || exit /b 0 | tee -a ${env.CONSOLE_LOG}"
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                bat "npm audit || exit /b 0 | tee -a ${env.CONSOLE_LOG}"
            }
        }
    }

    post {
        always {
            // Attach the captured console log to email
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
