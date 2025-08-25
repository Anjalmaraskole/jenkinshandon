pipeline {
    agent any
    environment {
        DOCKERHUB_CRED = credentials('6f41729b-b494-4b6b-a9de-a3c5f83730dc')
    }
    stages {
        stage('Build Docker Image') {
            steps {
                bat 'docker build -t anjali789/simple-web-apppp:$BUILD_NUMBER .'
            }
        }
        stage('Login to DockerHub') {
            steps {
                bat "echo $DOCKERHUB_CRED_PSW | docker login -u $DOCKERHUB_CRED_USR --password-stdin"
            }
        }
        stage('Push to DockerHub') {
            steps {
                bat 'docker push anjali789/simple-web-app:$BUILD_NUMBER'
            }
        }
    }
}
