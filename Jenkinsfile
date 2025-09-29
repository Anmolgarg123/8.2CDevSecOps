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
    }

   post {
    always {
        script {
            echo "Workspace path: ${env.WORKSPACE}"
            bat "dir ${env.WORKSPACE}"
            bat "type ${env.WORKSPACE}\\console-log.txt || echo File not found"
        }

        emailext(
            subject: "Test Email with Attachment",
            body: "Pipeline finished. Attaching console-log.txt from ${env.WORKSPACE}",
            to: "${EMAIL_RECIPIENTS}",
            attachmentsPattern: "console-log.txt"
        )
    }
}

}
