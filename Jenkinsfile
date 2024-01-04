pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
        S3_BUCKET_NAME = 'mytestbuckets2024'
        CLOUDFRONT_DISTRIBUTION_ID = 'E3GZDFJJVK0VCA'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // Add commands to build your static website (if needed)
            }
        }

        stage('Deploy to S3') {
            steps {
                script {
                    sh "aws s3 sync . s3://${S3_BUCKET_NAME}"
                }
            }
        }

        stage('Invalidate CloudFront Cache') {
            steps {
                script {
                    sh "aws cloudfront create-invalidation --distribution-id ${CLOUDFRONT_DISTRIBUTION_ID} --paths '/*'"
                }
            }
        }
    }
}
