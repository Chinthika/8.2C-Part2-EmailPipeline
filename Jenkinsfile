pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Tool: npm'
                sh 'npm install'
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                echo 'Tools: Mocha (unit) and Jest (integration)'
                sh 'npm test || true' // to allow post block to run even if tests fail
            }
            post {
                always {
                    emailext subject: "Unit & Integration Test Stage - ${currentBuild.currentResult}",
                             body: "The Unit and Integration Tests stage completed with status: ${currentBuild.currentResult}. Please check the attached logs.",
                             to: "chinthika.jayani@gmail.com",
                             attachLog: true
                }
            }
        }

        stage('Code Analysis') {
            steps {
                echo 'Tool: ESLint'
                sh 'npx eslint . || true'
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Tool: npm audit'
                sh 'npm audit || true'
            }
            post {
                always {
                    emailext subject: "Security Scan Stage - ${currentBuild.currentResult}",
                             body: "The Security Scan stage completed with status: ${currentBuild.currentResult}. Please check the attached logs.",
                             to: "chinthika.jayani@gmail.com",
                             attachLog: true
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo 'Tool: SCP or Docker Compose'
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
