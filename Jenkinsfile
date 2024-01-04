pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
        S3_BUCKET_NAME = 'mytestbuckets2024'
        CLOUDFRONT_DISTRIBUTION_ID = 'E3GZDFJJVK0VCA'
        AWS_CREDENTIALS_ID = 'ZaoJWA6ODvaYXNOd+cCOwY19W8bmt8Wnn0d9PHZF'
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
                    withCredentials([[
                        $class: 'AmazonWebServicesCredentialsBinding',
                        accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                        credentialsId: AWS_CREDENTIALS_ID,
                        secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
                    ]]) {
                        sh "aws s3 sync . s3://${S3_BUCKET_NAME} --delete --acl public-read --exclude 'Jenkinsfile'"
                    }
                }
            }
        }

        stage('Invalidate CloudFront Cache') {
            steps {
                script {
                    withCredentials([[
                        $class: 'AmazonWebServicesCredentialsBinding',
                        accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                        credentialsId: AWS_CREDENTIALS_ID,
                        secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
                    ]]) {
                        sh "aws configure set aws_access_key_id \$AWS_ACCESS_KEY_ID"
                        sh "aws configure set aws_secret_access_key \$AWS_SECRET_ACCESS_KEY"
                        sh "aws cloudfront create-invalidation --distribution-id ${CLOUDFRONT_DISTRIBUTION_ID} --paths '/*'"
                    }
                }
            }
        }
    }
}
