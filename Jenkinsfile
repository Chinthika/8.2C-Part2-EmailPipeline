pipeline {
    agent any

    environment {
        EMAIL_RECIPIENT = 'chinthika.jayani@gmail.com'
        LOG_FILE = 'build.log'
    }

    options {
        ansiColor('xterm')
    }

    stages {
        stage('Build') {
            steps {
                echo 'Stage: Build'
                sh '''
                    cd part2-task2-email
                    npm install | tee ../${LOG_FILE}
                '''
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                echo 'Stage: Unit and Integration Tests'
                script {
                    try {
                        sh '''
                            cd part2-task2-email
                            npm test | tee -a ../${LOG_FILE}
                        '''
                        currentBuild.result = 'SUCCESS'
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        throw e
                    }
                }
            }
            post {
                always {
                    emailext(
                        subject: "Test Stage: ${currentBuild.currentResult}",
                        body: "The Unit and Integration Test stage has completed with status: ${currentBuild.currentResult}",
                        to: "${EMAIL_RECIPIENT}",
                        attachmentsPattern: "${LOG_FILE}"
                    )
                }
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Stage: Security Scan'
                script {
                    try {
                        sh '''
                            cd part2-task2-email
                            npm audit | tee -a ../${LOG_FILE}
                        '''
                        currentBuild.result = 'SUCCESS'
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        throw e
                    }
                }
            }
            post {
                always {
                    emailext(
                        subject: "Security Scan: ${currentBuild.currentResult}",
                        body: "The Security Scan stage has completed with status: ${currentBuild.currentResult}",
                        to: "${EMAIL_RECIPIENT}",
                        attachmentsPattern: "${LOG_FILE}"
                    )
                }
            }
        }
    }
}
