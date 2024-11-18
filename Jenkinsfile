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
                withEnv([
                    "AWS_ACCESS_KEY_ID=${env.AWS_ACCESS_KEY_ID}",
                    "AWS_SECRET_ACCESS_KEY=${env.AWS_SECRET_ACCESS_KEY}",
                    "AWS_DEFAULT_REGION=${env.AWS_REGION}"
                ]) {
                    bat """
                    aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 985539792387.dkr.ecr.ap-south-1.amazonaws.com
                    docker build -t jenkins-demo .
                    docker tag jenkins-demo:latest 985539792387.dkr.ecr.ap-south-1.amazonaws.com/jenkins-demo:latest
                    docker push 985539792387.dkr.ecr.ap-south-1.amazonaws.com/jenkins-demo:latest
                    """
                }
            }
        }
    }
}
