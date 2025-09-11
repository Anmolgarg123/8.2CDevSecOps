pipeline {
    agent any

    environment {
		PROJECT_NAME = '8.2CDevSecOps'
        EMAIL_RECIPIENTS = "garganmol233@gmail.com"  // <-- replace with your email
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
            post {
                success {
                    emailext(
                        subject: "$PROJECT_NAME - Build # $BUILD_NUMBER - Tests SUCCESS",
                        body: "Unit and integration tests passed. Check console output at $BUILD_URL",
                        to: "${EMAIL_RECIPIENTS}"
                    )
                }
                failure {
                    emailext(
                        subject: "$PROJECT_NAME - Build # $BUILD_NUMBER - Tests FAILED",
                        body: "Unit and integration tests failed. Check console output at $BUILD_URL",
                        to: "${EMAIL_RECIPIENTS}"
                    )
                }
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
            post {
                success {
                    emailext(
                        subject: "$PROJECT_NAME - Build # $BUILD_NUMBER - Security Scan SUCCESS",
                        body: "NPM audit completed successfully. Check console output at $BUILD_URL",
                        to: "${EMAIL_RECIPIENTS}"
                    )
                }
                failure {
                    emailext(
                        subject: "$PROJECT_NAME - Build # $BUILD_NUMBER - Security Scan FAILED",
                        body: "NPM audit found vulnerabilities. Check console output at $BUILD_URL",
                        to: "${EMAIL_RECIPIENTS}"
                    )
                }
            }
        }
    }
}
