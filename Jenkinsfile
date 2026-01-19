pipeline {
    agent any

    environment {
        ECR_REPO = '596569258291.dkr.ecr.us-east-1.amazonaws.com/my-app'
        IMAGE_TAG = "${GIT_COMMIT}"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/username/my-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $ECR_REPO:$IMAGE_TAG .'
            }
        }

        stage('Push to ECR') {
            steps {
                sh '''
                aws ecr get-login-password --region us-east-1 | \
                docker login --username AWS --password-stdin 596569258291.dkr.ecr.us-east-1.amazonaws.com
                docker push $ECR_REPO:$IMAGE_TAG
                '''
            }
        }

        stage('Deploy to EKS') {
            steps {
                sh '''
                aws eks --region us-east-1 update-kubeconfig --name my-cluster
                kubectl set image deployment/my-app my-app=$ECR_REPO:$IMAGE_TAG --record
                '''
            }
        }
    }
}
