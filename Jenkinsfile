pipeline {
    agent any

    environment {
        LOG_FILE = 'build.log'
        RECIPIENT = 'chinthika.jayani@gmail.com'
    }

    stages {

        stage('Build') {
            steps {
                echo 'Tool: npm (for JavaScript/Node.js projects)'
                sh 'npm install > ${LOG_FILE} 2>&1 || true'
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                echo 'Tools: Jest (for unit testing), Supertest (for integration tests)'
                script {
                    def status = sh(script: 'npm test >> ${LOG_FILE} 2>&1', returnStatus: true)
                    if (status == 0) {
                        emailext (
                            to: "${env.RECIPIENT}",
                            subject: "Unit & Integration Tests Succeeded",
                            body: "The Unit and Integration Tests stage completed successfully.",
                            attachmentsPattern: "${LOG_FILE}",
                            mimeType: 'text/plain'
                        )
                    } else {
                        emailext (
                            to: "${env.RECIPIENT}",
                            subject: "Unit & Integration Tests Failed",
                            body: "The Unit and Integration Tests stage failed. Please check the attached log.",
                            attachmentsPattern: "${LOG_FILE}",
                            mimeType: 'text/plain'
                        )
                        error("Tests failed.")
                    }
                }
            }
        }

        stage('Code Analysis') {
            steps {
                echo 'Tool: SonarQube (for static code analysis)'
                echo 'Pretend we run sonar-scanner here.'
                sh 'echo "SonarQube analysis complete." >> ${LOG_FILE}'
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Tool: Snyk (for vulnerability scanning in dependencies)'
                script {
                    def status = sh(script: 'echo "Running Snyk scan..." >> ${LOG_FILE} && snyk test >> ${LOG_FILE} 2>&1', returnStatus: true)
                    if (status == 0) {
                        emailext (
                            to: "${env.RECIPIENT}",
                            subject: "Security Scan Passed",
                            body: "Security scan completed with no vulnerabilities.",
                            attachmentsPattern: "${LOG_FILE}",
                            mimeType: 'text/plain'
                        )
                    } else {
                        emailext (
                            to: "${env.RECIPIENT}",
                            subject: "Security Scan Failed",
                            body: "Security scan found issues. Please see the attached log.",
                            attachmentsPattern: "${LOG_FILE}",
                            mimeType: 'text/plain'
                        )
                        error("Security scan failed.")
                    }
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo 'Tool: Docker Compose or Helm (for staging deployment)'
                echo 'Pretend we deploy to staging.'
                sh 'echo "Deployed to staging." >> ${LOG_FILE}'
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo 'Tool: Selenium (for UI/end-to-end testing)'
                echo 'Pretend we run Selenium tests.'
                sh 'echo "Selenium tests passed." >> ${LOG_FILE}'
            }
        }

        stage('Deploy to Production') {
            steps {
                echo 'Tool: AWS CLI (for deployment to AWS EC2)'
                echo 'Pretend we deploy to production.'
                sh 'echo "Deployed to production." >> ${LOG_FILE}'
            }
        }
    }
}
