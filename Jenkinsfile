pipeline {
    agent any

    triggers {
        // Configure webhook or polling here
        pollSCM('* * * * *') // Poll every minute
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building...'
                // Add your build steps here
            }
        }

    }
}
