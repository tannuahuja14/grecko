pipeline {
    agent any

    environment {
        DOCKER_REGISTRY = "my-docker-app"
        DOCKER_IMAGE_NAME = "my-app"
        KUBECONFIG = credentials('your-kubeconfig-credentials-id')
    }

    stages {
        stage('Build/Test') {
            steps {
                sh 'echo "Build and test the application"'
            }
        }

        stage('Generate Artifacts') {
            steps {
                sh 'echo "Generate artifacts"'
            }
        }

        stage('Dockerize') {
            steps {
                sh 'docker build -t $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:$GIT_COMMIT .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                sh 'docker push $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:$GIT_COMMIT'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl config use-context minikube'
                sh 'kubectl apply -f kubernetes/deployment.yaml'
                sh 'kubectl rollout status deployment/my-app'
            }
        }
    }
}
