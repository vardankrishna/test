pipeline {
    agent any

    parameters {
        booleanParam(name: 'QA_APPROVAL', defaultValue: false, description: 'QA Approval')
        booleanParam(name: 'STAGING_APPROVAL', defaultValue: false, description: 'Staging Approval')
        booleanParam(name: 'PRODUCTION_APPROVAL', defaultValue: false, description: 'Production Approval')
    }

    options {
        buildDiscarder(logRotator(artifactNumToKeepStr: '5', numToKeepStr: '5'))
    }

    triggers {
        pollSCM('* * * * *') // Poll every day
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building...'
                // Add your build steps here
            }
        }

        stage('Deploy to Dev') {
            when {
                expression { true }
            }
            steps {
                echo 'Deploying to Dev...'
                // Add your deployment steps for Dev environment here
            }
        }

        stage('Deploy to QA') {
            when {
                expression { params.QA_APPROVAL }
            }
            steps {
                script {
                    def qaApproval = input(
                        id: 'qa-approval',
                        message: 'Do you want to promote this build to QA?',
                        submitter: 'qa-team',
                        parameters: [
                            [$class: 'TextParameterDefinition', defaultValue: '', description: 'Enter your approval message:', name: 'ApprovalMessage']
                        ]
                    )
                    echo "QA Approval: ${qaApproval}"
                }
                echo 'Deploying to QA...'
                // Add your deployment steps for QA environment here
            }
        }

        stage('Deploy to Staging') {
            when {
                expression { params.STAGING_APPROVAL }
            }
            steps {
                script {
                    def stagingApproval = input(
                        id: 'staging-approval',
                        message: 'Do you want to promote this build to Staging?',
                        submitter: 'staging-team',
                        parameters: [
                            [$class: 'TextParameterDefinition', defaultValue: '', description: 'Enter your approval message:', name: 'ApprovalMessage']
                        ]
                    )
                    echo "Staging Approval: ${stagingApproval}"
                }
                echo 'Deploying to Staging...'
                // Add your deployment steps for Staging environment here
            }
        }

        stage('Deploy to Production') {
            when {
                expression { params.PRODUCTION_APPROVAL }
            }
            steps {
                script {
                    def productionApproval = input(
                        id: 'production-approval',
                        message: 'Do you want to promote this build to Production?',
                        submitter: 'production-team',
                        parameters: [
                            [$class: 'TextParameterDefinition', defaultValue: '', description: 'Enter your approval message:', name: 'ApprovalMessage']
                        ]
                    )
                    echo "Production Approval: ${productionApproval}"
                }
                echo 'Deploying to Production...'
                // Add your deployment steps for Production environment here
            }
        }
    }

    post {
        failure {
            emailext body: 'The build failed. Please check the Jenkins build logs for details.',
                    subject: 'Build Failed',
                    to: 'your-email@example.com'
        }

        success {
            emailext body: 'The build was successful. Deployment can proceed.',
                    subject: 'Build Successful',
                    to: 'your-email@example.com'
        }
    }
}
