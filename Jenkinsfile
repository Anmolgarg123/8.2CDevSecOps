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
                echo "Tool: Git"
                git branch: 'main', url: 'https://github.com/Anmolgarg123/8.2CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo "Tool: npm"
                bat """
                @echo off
                echo ===== Install Dependencies ===== > console-log.txt
                npm install >> console-log.txt 2>&1
                """
            }
        }

        stage('Run Tests') {
            steps {
                echo "Tool: npm test"
                bat """
                @echo off
                echo ===== Run Tests ===== >> console-log.txt
                npm test || exit /b 0 >> console-log.txt 2>&1
                """
            }
            post {
                always {
                    emailext(
                        subject: "${PROJECT_NAME} - Run Tests Stage - ${currentBuild.currentResult}",
                        body: "Run Tests stage finished. See attached log.",
                        to: "${EMAIL_RECIPIENTS}",
                        attachmentsPattern: 'console-log.txt'
                    )
                }
            }
        }

        stage('Generate Coverage Report') {
            steps {
                echo "Tool: npm run coverage"
                bat """
                @echo off
                echo ===== Coverage Report ===== >> console-log.txt
                npm run coverage || exit /b 0 >> console-log.txt 2>&1
                """
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                echo "Tool: npm audit"
                bat """
                @echo off
                echo ===== Security Scan ===== >> console-log.txt
                npm audit || exit /b 0 >> console-log.txt 2>&1
                """
            }
            post {
                always {
                    emailext(
                        subject: "${PROJECT_NAME} - Security Scan Stage - ${currentBuild.currentResult}",
                        body: "Security Scan stage finished. See attached log.",
                        to: "${EMAIL_RECIPIENTS}",
                        attachmentsPattern: 'console-log.txt'
                    )
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline finished for build #${BUILD_NUMBER}"
        }
    }
}
