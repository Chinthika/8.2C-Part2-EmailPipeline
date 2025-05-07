pipeline {
    agent any

    environment {
        LOG_FILE = 'pipeline-stage.log'
    }

    stages {
        stage('Build') {
            steps {
                echo 'Tool: npm'
                sh 'npm install'
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                echo 'Tools: JUnit (unit) and Testcontainers (integration)'
                sh 'echo "Running tests..." > ${LOG_FILE}'
                sh 'npm test || echo "Tests failed but continuing" >> ${LOG_FILE}'
            }
            post {
                always {
                    emailext subject: "Test Stage - Build ${currentBuild.currentResult}",
                              body: "The Test stage has completed with status: ${currentBuild.currentResult}. Please find the attached logs.",
                              to: "chinthika.jayani@gmail.com",
                              attachmentsPattern: "${LOG_FILE}",
                              compressLog: true
                }
            }
        }

        stage('Code Analysis') {
            steps {
                echo 'Tool: SonarQube'
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Tool: Snyk'
                sh 'echo "Running npm audit..." > ${LOG_FILE}'
                sh 'npm audit || echo "Security scan found issues" >> ${LOG_FILE}'
            }
            post {
                always {
                    emailext subject: "Security Scan - Build ${currentBuild.currentResult}",
                              body: "The Security Scan stage has completed with status: ${currentBuild.currentResult}. Please find the attached logs.",
                              to: "chinthika.jayani@gmail.com",
                              attachmentsPattern: "${LOG_FILE}",
                              compressLog: true
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo 'Tool: Helm (for Kubernetes deployment via AWS EKS)'
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo 'Tool: Selenium'
            }
        }

        stage('Deploy to Production') {
            steps {
                echo 'Tool: AWS CLI'
            }
        }
    }
}
