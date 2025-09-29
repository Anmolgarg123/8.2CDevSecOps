pipeline {
    agent any

    environment {
        PROJECT_NAME = '8.2CDevSecOps'
        EMAIL_RECIPIENTS = "garganmol233@gmail.com"
        CONSOLE_LOG = "${WORKSPACE}/console-log.txt"
    }

    stages {
        stage('Checkout') {
            steps {
                echo "===== Checkout =====" >> "${CONSOLE_LOG}"
                git branch: 'main', url: 'https://github.com/Anmolgarg123/8.2CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo "===== Install Dependencies =====" >> "${CONSOLE_LOG}"
                bat """
                @echo off
                npm install >> ${CONSOLE_LOG} 2>&1
                """
            }
        }

        stage('Run Tests') {
            steps {
                echo "===== Run Tests =====" >> "${CONSOLE_LOG}"
                bat """
                @echo off
                npm test || exit /b 0 >> ${CONSOLE_LOG} 2>&1
                """
            }
        }

        stage('Generate Coverage Report') {
            steps {
                echo "===== Coverage Report =====" >> "${CONSOLE_LOG}"
                bat """
                @echo off
                npm run coverage || exit /b 0 >> ${CONSOLE_LOG} 2>&1
                """
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                echo "===== Security Scan =====" >> "${CONSOLE_LOG}"
                bat """
                @echo off
                npm audit || exit /b 0 >> ${CONSOLE_LOG} 2>&1
                """
            }
        }
    }

    post {
        always {
            echo "Pipeline finished for build #${BUILD_NUMBER}"

            // Send email with attachment
            emailext(
                subject: "${PROJECT_NAME} - Build #${BUILD_NUMBER} - ${currentBuild.currentResult}",
                body: "Pipeline finished. See attached console log for details.",
                to: "${EMAIL_RECIPIENTS}",
                attachmentsPattern: "${CONSOLE_LOG}"
            )
        }
    }
}
