pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = "us-east-1"
        S3_BUCKET = "your-jenkins-builds"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/preethichowdaryb/samplerepo.git'
            }
        }

        stage('Upload to S3') {
            steps {
                withAWS(credentials: 'AWS conf with jenkins', region: 'us-east-1') {
                    sh 'aws s3 cp sample.txt s3://your-jenkins-builds/sample.txt'
                }
            }
        }
    }

    post {
        success {
            echo "✅ File successfully uploaded to S3!"
        }
        failure {
            echo "❌ Failed to upload to S3."
        }
    }
}

