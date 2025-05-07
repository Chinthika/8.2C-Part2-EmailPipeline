pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Tool: npm'
                sh 'echo "Simulated build complete." > build.log'
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                script {
                    echo 'Tools: Mocha (unit) and Jest (integration)'
                    try {
                        sh 'npm test'
                        currentBuild.result = 'SUCCESS'
                        echo 'Tests passed successfully.'
                    } catch (err) {
                        currentBuild.result = 'FAILURE'
                        echo 'Tests failed.'
                    }
                }
            }
            post {
                always {
                    emailext(
                        subject: "Unit & Integration Test Stage - ${currentBuild.result}",
                        body: "The Unit and Integration Tests stage completed with status: ${currentBuild.result}. Please check the attached logs.",
                        to: "s225045772@deakin.edu.au",
                        attachLog: true
                    )
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
                script {
                    echo 'Tool: npm audit'
                    try {
                        sh 'npm audit'
                        currentBuild.result = 'SUCCESS'
                        echo 'Security scan passed. No vulnerabilities found.'
                    } catch (err) {
                        currentBuild.result = 'FAILURE'
                        echo 'Security scan failed. Vulnerabilities detected.'
                    }
                }
            }
            post {
                always {
                    emailext(
                        subject: "Security Scan Stage - ${currentBuild.result}",
                        body: "The Security Scan stage completed with status: ${currentBuild.result}. Please check the attached logs.",
                        to: "s225045772@deakin.edu.au",
                        attachLog: true
                    )
                }
            }
        }
        
        stage('Deploy to Staging') {
            steps {
                echo 'Tool: Docker Compose'
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

