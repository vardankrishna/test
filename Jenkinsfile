pipeline {
    agent any

    options {
        buildDiscarder(logRotator(artifactNumToKeepStr: '5', numToKeepStr: '5'))
    }

    triggers {
        githubPush() // Automatically triggered by webhooks on GitHub push events
    }

    def changesetExistsInMain() {
        return env.BRANCH_NAME == 'main' && currentBuild.changeSets.size() > 0
    }

    def changesetExistsInRelease() {
        return env.BRANCH_NAME == 'release-20231226' && currentBuild.changeSets.size() > 0
    }

    stages {
        stage('Build') {
            when {
                expression { env.BRANCH_NAME == 'main' || env.BRANCH_NAME == 'release-20231226' }
            }
            steps {
                echo 'Building...'
                // Add your build steps here
            }
        }

        stage('Deploy to Dev') {
            when {
                expression { changesetExistsInMain() }
            }
            steps {
                echo 'Deploying to Dev...'
                // Add your deployment steps for Dev environment here
            }
        }

        stage('Deploy to QA') {
            when {
                expression { changesetExistsInRelease() }
            }
            steps {
                echo 'Deploying to QA...'
                // Add your deployment steps for QA environment here
            }
        }

        stage('Deploy to Production') {
            input {
                message 'Do you want to deploy to Production?'
                ok 'Deploy'
            }
            steps {
                echo 'Deploying to Production...'
                // Add your deployment steps for Production environment here
            }
        }
    }

    post {
        failure {
            emailext body: 'The build failed. Please check the Jenkins build logs for details.',
                    subject: 'Build Failed',
                    to: 'vamshikrishnadontham@gmail.com'
        }

        success {
            emailext body: 'The build was successful. Deployment can proceed.',
                    subject: 'Build Successful',
                    to: 'vamshikrishnadontham@gmail.com'
        }
    }
}
