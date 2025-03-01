│   ├── pipeline {
│   agent any
│   environment {
│       IMAGE_NAME = 'your-dockerhub-username/app'
│       GKE_CLUSTER = 'your-cluster-name'
│       GKE_ZONE = 'your-cluster-zone'
│       K8S_NAMESPACE = 'default'
│   }
│   stages {
│       stage('Checkout') {
│           steps {
│               checkout scm
│           }
│       }
│       stage('Build Docker Image') {
│           steps {
│               sh 'docker build -t $IMAGE_NAME:latest ./app'
│           }
│       }
│       stage('Push to Docker Hub') {
│           steps {
│               sh 'docker login -u your-dockerhub-username -p your-dockerhub-password'
│               sh 'docker push $IMAGE_NAME:latest'
│           }
│       }
│       stage('Deploy to GKE') {
│           steps {
│               sh 'gcloud auth configure-docker'
│               sh 'gcloud container clusters get-credentials $GKE_CLUSTER --zone $GKE_ZONE'
│               sh 'kubectl apply -f k8s/'
│           }
│       }
│   }
│}
