pipeline {
    agent any

    environment {
        IMAGE_NAME = "adimane0801/nginx-probe"
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {

       /* stage('Checkout Code') {
            steps {
                git 'https://github.com/adimane-08/k8s-.git'
            }
        } */

        stage('Build Docker Image') {
            steps {
                sh "docker build -t $IMAGE_NAME:$IMAGE_TAG ."
            }
        }

        stage('Push Docker Image') {
            steps {
                sh "docker push $IMAGE_NAME:$IMAGE_TAG"
            }
        }

        stage('Update Deployment Image') {
            steps {
                sh """
                kubectl set image deployment/nginx-probe nginx=$IMAGE_NAME:$IMAGE_TAG
                """
            }
        }
    }
}
