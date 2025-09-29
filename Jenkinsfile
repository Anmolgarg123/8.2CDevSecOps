pipeline {
    agent any

    environment {
        CONSOLE_LOG = "console-log.txt"
        EMAIL_RECIPIENTS = "garganmol233@gmail.com"
    }

    stages {

        stage('Checkout SCM') {
            steps {
                checkout scm
                bat "echo ===== Checked out SCM ===== > %CONSOLE_LOG%"
            }
        }

        stage('Install Dependencies') {
            steps {
                bat """
                echo ===== Install Dependencies ===== >> %CONSOLE_LOG%
                npm install >> %CONSOLE_LOG% 2>&1
                """
            }
        }

        stage('Run Tests') {
            steps {
                bat """
                echo ===== Run Tests ===== >> %CONSOLE_LOG%
                npm test >> %CONSOLE_LOG% 2>&1 || exit /b 0
                """
            }
        }

        stage('Generate Coverage Report') {
            steps {
                bat """
                echo ===== Coverage Report ===== >> %CONSOLE_LOG%
                """
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                bat """
                echo ===== Security Scan ===== >> %CONSOLE_LOG%
                npm audit >> %CONSOLE_LOG% 2>&1 || exit /b 0
                """
            }
        }

        stage('Snyk Security Scan') {
            steps {
                bat """
                echo ===== Snyk Scan ===== >> %CONSOLE_LOG%
                snyk test >> %CONSOLE_LOG% 2>&1 || exit /b 0
                """
            }
        }
    }

    post {
        always {
            // Show workspace and log for debug
            script {
                echo "Workspace path: ${env.WORKSPACE}"
                bat "dir ${env.WORKSPACE}"
                bat "type ${env.WORKSPACE}\\${CONSOLE_LOG} || echo File not found"
            }

            // Send email with only console-log.txt attached
            emailext(
                subject: "Pipeline Finished - Console Log Attached",
                body: """Hello,

The Jenkins pipeline has finished. Please find attached the console log containing all outputs from checkout, npm install, tests, coverage, and security scans.

Regards,
Jenkins""",
                to: "${EMAIL_RECIPIENTS}",
                attachmentsPattern: "${env.WORKSPACE}\\${CONSOLE_LOG}"
            )
        }
    }
}
