pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                script {
                    echo 'Building the application'
                    // Add your build commands here
                }
            }
        }

        stage('Deploy') {
            when {
                expression { 
                    // Trigger for changes in feature branch
                    env.BRANCH_NAME.startsWith('FEATURE/') 
                }
            }
            steps {
                script {
                     bat 'copy C:\\Users\\vamshi\\.jenkins\\workspace\\pipeline C:\\inetpub\\wwwroot\\sample'
                    // Add your deployment commands for dev environment here
                }
            }
        }

        stage('Deploy to QA') {
            when {
                expression { 
                    // Trigger for changes in development branch
                    env.BRANCH_NAME == 'DEVELOPMENT'
                }
            }
            steps {
                script {
                     bat 'copy C:\\Users\\vamshi\\.jenkins\\workspace\\pipeline C:\\inetpub\\wwwroot\\sample'
                    // Add your deployment commands for QA environment here
                }
            }
        }

        stage('Deploy to Production') {
            when {
                expression { 
                    // Trigger for changes in master branch
                    env.BRANCH_NAME == 'master' || env.CHANGE_TARGET == 'master'
                }
            }
            steps {
                script {
                     bat 'copy C:\\Users\\vamshi\\.jenkins\\workspace\\pipeline C:\\inetpub\\wwwroot\\sample'
                    // Add your deployment commands for production environment here
                }
            }
        }
    }

    post {
        success {
            emailext subject: 'CI/CD Pipeline Success',
                      body: 'The CI/CD pipeline has succeeded!',
                      recipientProviders: [[$class: 'CulpritsRecipientProvider'],
                                           [$class: 'RequesterRecipientProvider']],
                      to: 'vamshikrishnadontham@gmail.com'
        }
        failure {
            emailext subject: 'CI/CD Pipeline Failure',
                      body: 'The CI/CD pipeline has failed. Please check the build logs for details.',
                      recipientProviders: [[$class: 'CulpritsRecipientProvider'],
                                           [$class: 'RequesterRecipientProvider']],
                      to: 'vamshikrishnadontham@gmail.com'
        }
    }
}
