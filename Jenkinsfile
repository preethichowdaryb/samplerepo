pipeline {
    agent any

    environment {
        S3_BUCKET = "your-jenkins-builds"
    }

    triggers {
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
                    sh "aws s3 cp ${WORKSPACE}/sample.txt s3://${S3_BUCKET}/sample.txt --acl private --exact-timestamps"
                }
            }
        }
    }
}

