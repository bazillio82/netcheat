pipeline {
    agent any

    environment {
        S3_BUCKET = 'netcheat-oleksii-bondar-2026-862939497077'
        AWS_REGION = 'eu-central-1'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Validate HTML') {
            steps {
                sh '''
                    echo "=== HTML validation ==="
                    htmlhint index.html error.html
                '''
            }
        }

        stage('Check Shpargalka') {
            steps {
                sh '''
                    echo "=== Required content check ==="
                    grep -q "Шпаргалка" index.html
                    echo "Required text found"
                '''
            }
        }

        stage('Manual Approval') {
            steps {
                input message: 'Deploy Netcheat to production S3?', ok: 'Deploy'
            }
        }

        stage('Deploy to S3') {
            steps {
                sh '''
                    echo "=== Deploying to S3 ==="

                    aws s3 cp index.html \
                      s3://$S3_BUCKET/index.html \
                      --region $AWS_REGION \
                      --content-type text/html

                    aws s3 cp error.html \
                      s3://$S3_BUCKET/error.html \
                      --region $AWS_REGION \
                      --content-type text/html

                    echo "Deployment completed"
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
                    echo "=== S3 contents ==="
                    aws s3 ls s3://$S3_BUCKET/ --region $AWS_REGION
                '''
            }
        }
    }

    post {
        success {
            echo 'Netcheat CI/CD pipeline completed successfully.'
        }

        failure {
            echo 'Pipeline failed. Production deployment was not completed.'
        }
    }
}