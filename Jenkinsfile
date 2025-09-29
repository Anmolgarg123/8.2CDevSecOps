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
                    script {
                        // Capture Jenkins console output into a file
                        writeFile file: "${CONSOLE_LOG}", text: currentBuild.rawBuild.getLog(1000).join('\n')
                    }
                    // Send email with attachment
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
                echo "Tool: npm audit (Security Scan)"
                bat 'npm audit || exit /b 0'
            }
            post {
                always {
                    script {
                        // Capture Jenkins console output into a file
                        writeFile file: "${CONSOLE_LOG}", text: currentBuild.rawBuild.getLog(1000).join('\n')
                    }
                    // Send email with attachment
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
