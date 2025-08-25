pipeline {
    agent any
    environment {
        DOCKERHUB_CRED = credentials('dockerhub')
    }
    stages {
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t anjali789/simple-web-apppp:$BUILD_NUMBER .'
            }
        }
        stage('Login to DockerHub') {
            steps {
                sh "echo $DOCKERHUB_CRED_PSW | docker login -u $DOCKERHUB_CRED_USR --password-stdin"
            }
        }
        stage('Push to DockerHub') {
            steps {
                sh 'docker push anjali789/simple-web-app:$BUILD_NUMBER'
            }
        }
    }
}
