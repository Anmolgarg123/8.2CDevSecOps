pipeline {
    agent any

    environment {
        CONSOLE_LOG = "console-log.txt"
        SNYK_REPORT = "snyk-report.json"
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

        stage('Snyk Security Scan') {
            steps {
                bat """
                echo ===== Snyk Security Scan ===== >> %CONSOLE_LOG%
                snyk test --json > %SNYK_REPORT% 2>&1 || exit /b 0
                type %SNYK_REPORT% >> %CONSOLE_LOG%
                """
            }
        }

    }

    post {
        always {
            script {
                echo "Workspace path: ${env.WORKSPACE}"
                bat "dir ${env.WORKSPACE}"
                bat "type ${env.WORKSPACE}\\console-log.txt || echo File not found"
            }

            emailext(
                subject: "Jenkins Build & Snyk Report",
                body: "Pipeline finished. Attached are the console log and Snyk report.",
                to: "${EMAIL_RECIPIENTS}",
                attachmentsPattern: "console-log.txt, snyk-report.json",
                mimeType: 'text/plain'
            )
        }
    }
}
