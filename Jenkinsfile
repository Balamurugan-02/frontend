pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
        S3_BUCKET = 'amzn-nextgen'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/jyothikaalla15/project-praticals-.git'
            }
        }

        stage('Verify Node') {
            steps {
                sh '''
                  node -v
                  npm -v
                '''
            }
        }

        stage('Build React') {
            steps {
                sh '''
                  npm install
                  npm run build
                '''
            }
        }

        stage('Deploy to S3 & Invalidate CloudFront') {
            steps {
                withAWS(credentials: 'aws-credentials', region: 'us-east-1') {
                    sh '''
                      aws s3 sync build/ s3://${S3_BUCKET} --delete
                      aws cloudfront create-invalidation \
                        --distribution-id E1QEK1LS9J2AK1 \
                        --paths "/*"
                    '''
                }
            }
        }
    }
}
