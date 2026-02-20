pipeline {
    agent any
    environment {
        DOCKERHUB = 'adimane-08'
        IMAGE = 'nginx-probe'
        TAG = 'latest'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'feature', url: 'https://github.com/adimane-08/k8s-.git', credentialsId: 'code-for-k8s'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh "docker build -t $DOCKERHUB/$IMAGE:$TAG ."
            }
        }
        stage('Push Docker Image') {
            steps {
                withDockerRegistry([ credentialsId: 'dockerhub-cred', url: '' ]) {
                    sh "docker push $DOCKERHUB/$IMAGE:$TAG"
                }
            }
        }
        stage('Update Deployment') {
            steps {
                sh "kubectl set image deployment/nginx-probe nginx=$DOCKERHUB/$IMAGE:$TAG"
            }
        }
    }
    post {
        success {
            echo 'Deployment updated successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
