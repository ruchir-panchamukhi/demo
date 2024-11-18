pipeline {
    agent any

    environment {
        AWS_ACCOUNT_ID = '985539792387'
        AWS_REGION = 'ap-south-1' 
        ECR_REPO_NAME = 'jenkins-demo' 
        IMAGE_TAG = 'latest' 
    }

    stages {
        stage('Build Docker Image') {
            steps {
                bat 'docker build -t jenkins-demo .'
            }
        }

        stage('Publish to ECR') {
            steps {
                withCredentials([string(credentialsId: 'aws-access-key-id', variable: 'AWS_ACCESS_KEY_ID'),
                                 string(credentialsId: 'aws-secret-access-key', variable: 'AWS_SECRET_ACCESS_KEY')]) {
                    withEnv([ 
                        "AWS_ACCESS_KEY_ID=${env.AWS_ACCESS_KEY_ID}", 
                        "AWS_SECRET_ACCESS_KEY=${env.AWS_SECRET_ACCESS_KEY}",
                        "AWS_DEFAULT_REGION=${env.AWS_REGION}"
                    ]) {
                        bat """
                        aws ecr get-login-password --region ${env.AWS_REGION} | docker login --username AWS --password-stdin ${env.AWS_ACCOUNT_ID}.dkr.ecr.${env.AWS_REGION}.amazonaws.com
                        docker tag jenkins-demo:latest ${env.AWS_ACCOUNT_ID}.dkr.ecr.${env.AWS_REGION}.amazonaws.com/${env.ECR_REPO_NAME}:${env.IMAGE_TAG}
                        docker push ${env.AWS_ACCOUNT_ID}.dkr.ecr.${env.AWS_REGION}.amazonaws.com/${env.ECR_REPO_NAME}:${env.IMAGE_TAG}
                        """
                    }
                }
            }
        }
    }
}
