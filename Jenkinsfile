pipeline {
    agent { label 'master' }
    environment {
        IMAGE_NAME = 'raja250/practice/app'
        GKE_CLUSTER = 'cluster-1'
        GKE_ZONE = 'us-central1-c'
        K8S_NAMESPACE = 'default'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t $IMAGE_NAME:latest ./app'
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'Docker', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        // Log in to Docker Hub
                        sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
                        // Push the image to Docker Hub
                        sh 'docker push $IMAGE_NAME:latest'
                    }
                }
            }
        }

        stage('Deploy to GKE') {
            steps {
                script {
                    withCredentials([file(credentialsId: 'Gcloud', variable: 'GOOGLE_APPLICATION_CREDENTIALS')]) {
                        // Authenticate GCP service account and configure Docker for GKE
                        sh 'gcloud auth activate-service-account --key-file=$GOOGLE_APPLICATION_CREDENTIALS'
                        sh 'gcloud auth configure-docker'
                        // Get GKE credentials
                        sh 'gcloud container clusters get-credentials $GKE_CLUSTER --zone $GKE_ZONE'
                        // Deploy to Kubernetes
                        sh 'kubectl apply -f k8s/'
                    }
                }
            }
        }
    }
}
