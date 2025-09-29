pipeline {
    agent any

    environment {
        PROJECT_NAME = '8.2CDevSecOps'
        EMAIL_RECIPIENTS = "garganmol233@gmail.com"
        WORKSPACE_LOG = "${WORKSPACE}\\console-log.txt"
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
                bat 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                echo "Tool: npm test"
                bat 'npm test || exit /b 0'
            }
            post {
                always {
                    // Capture stage output into a log
                    bat """
                        @echo off
                        echo ===== Run Tests Stage Log ===== > "${WORKSPACE_LOG}"
                        for /r "%WORKSPACE%" %%f in (*.log) do type "%%f" >> "${WORKSPACE_LOG}"
                    """
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
                bat 'npm run coverage || exit /b 0'
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                echo "Tool: npm audit (Snyk optional)"
                bat 'npm audit || exit /b 0'
            }
            post {
                always {
                    // Capture security scan log
                    bat """
                        @echo off
                        echo ===== Security Scan Stage Log ===== > "${WORKSPACE_LOG}"
                        for /r "%WORKSPACE%" %%f in (*.log) do type "%%f" >> "${WORKSPACE_LOG}"
                    """
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
