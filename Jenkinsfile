pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
        S3_BUCKET = 'shiva-169237360288-us-east-1-an'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Balamurugan-02/frontend.git'
            }
        }

        stage('Deploy to S3') {
            steps {
                withAWS(credentials: 'aws-credentials', region: 'us-east-1') {
                    sh '''
                        aws s3 sync . s3://${S3_BUCKET} --delete --exclude ".git/*"
                    '''
                }
            }
        }

        stage('Invalidate CloudFront') {
            steps {
                withAWS(credentials: 'aws-credentials', region: 'us-east-1') {
                    sh '''
                        aws cloudfront create-invalidation \
                        --distribution-id EZUNXXB9FS9PY \
                        --paths "/*"
                    '''
                }
            }
        }
    }
}
