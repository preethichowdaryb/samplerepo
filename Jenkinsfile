pipeline {
    agent any

    environment {
        S3_BUCKET = "your-jenkins-builds"  // ✅ Your actual S3 bucket
    }

    triggers {
        githubPush()   // ✅ Auto-trigger on GitHub webhook push event
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo "📥 Checking out code from GitHub..."
                git branch: 'main', url: 'https://github.com/preethichowdaryb/samplerepo.git'
            }
        }

        stage('Check for sample.txt changes') {
            steps {
                script {
                    // ✅ Find which files changed in this commit
                    def changedFiles = sh(script: "git diff-tree --no-commit-id --name-only -r HEAD", returnStdout: true).trim()
                    echo "Changed files: ${changedFiles}"

                    if (!changedFiles.contains('sample.txt')) {
                        echo "✅ No changes in sample.txt. Skipping upload."
                        currentBuild.result = 'SUCCESS'
                        error("No upload needed")
                    } else {
                        echo "✅ Detected changes in sample.txt. Proceeding to upload."
                    }
                }
            }
        }

        stage('Upload to S3') {
            steps {
                echo "🚀 Uploading sample.txt to S3 bucket: ${env.S3_BUCKET}"
                withAWS(region: 'us-east-1', credentials: 'AWS conf with jenkins') {
                    sh "aws s3 cp sample.txt s3://${S3_BUCKET}/sample.txt"
                }
            }
        }
    }

    post {
        success {
            echo "✅ Successfully uploaded sample.txt to S3 bucket: ${env.S3_BUCKET}"
        }
        failure {
            echo "❌ Failed to upload sample.txt to S3."
        }
    }
}
