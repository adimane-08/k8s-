pipeline {
    agent any

    environment {
        IMAGE_NAME = "adimane-08/nginx-probe"  // Replace with your DockerHub repo
        IMAGE_TAG = "${BUILD_NUMBER}"
        KUBECONFIG = "C:\\Users\\Admin\\.kube\\config"  // Path to your kubeconfig
    }

    stages {
        stage('Build Docker Image') {
            steps {
                echo "Building Docker image..."
                bat "docker build -t %IMAGE_NAME%:%IMAGE_TAG% ."
            }
        }

        stage('Push Docker Image') {
            steps {
                echo "Pushing Docker image to DockerHub..."
                bat "docker login -u your_dockerhub_username -p your_dockerhub_password"
                bat "docker push %IMAGE_NAME%:%IMAGE_TAG%"
            }
        }

        stage('Update Deployment') {
            steps {
                echo "Updating Kubernetes deployment..."
                // Option 1: update image directly
                bat "kubectl set image -f nginx-probe.yaml nginx=%IMAGE_NAME%:%IMAGE_TAG%"

                // Option 2: apply the whole YAML if you have other changes
                // bat "kubectl apply -f nginx-probe.yaml"
            }
        }
    }

    post {
        always {
            echo "Cleaning workspace..."
            cleanWs()
        }
    }
}
