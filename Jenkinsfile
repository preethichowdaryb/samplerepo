pipeline {
    agent any

    triggers {
        // This makes Jenkins listen for GitHub webhook events
        githubPush()
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/preethichowdaryb/samplerepo.git'
            }
        }

        stage('Upload to S3') {
            steps {
                withAWS(region: 'us-east-1', credentials: 'AWS conf with jenkins') {
                    // Full AWS CLI path to avoid PATH issues
                    sh '/opt/homebrew/bin/aws s3 cp sample.txt s3://your-jenkins-builds/sample.txt'
                }
            }
        }
    }

    post {
        success {
            echo " Successfully uploaded to S3."
        }
        failure {
            echo " Failed to upload to S3."
        }
    }
}


