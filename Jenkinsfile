pipeline {
  agent any
  
  environment {
    DOCKER_REGISTRY = 'my-docker-app'
    DOCKER_IMAGE_NAME = 'my-app'
    KUBECONFIG = credentials('your-kubeconfig-credentials-id')
  }

  stages {
    stage('Build') {
      steps {
        sh 'echo "Building HTML/CSS project..."'
        sh 'mkdir -p build'
        sh 'cp -r src/* build'
      }
    }

    stage('Test') {
      steps {
        sh 'echo "Running tests..."'
        sh 'echo "No tests found."'
      }
    }

    stage('Package') {
      steps {
        sh 'echo "Packaging application..."'
        sh 'docker build -t my-docker-repo/my-app:$GIT_COMMIT .'
        sh 'docker push my-docker-repo/my-app:$GIT_COMMIT'

      }
    }

    stage('Deploy') {
      steps {
        sh 'echo "Deploying to Kubernetes..."'
        withCredentials([kubeconfigFile(credentialsId: 'your-kubeconfig-credentials-id', variable: 'KUBECONFIG')]) {
          sh 'kubectl config use-context minikube'
          sh 'kubectl set image deployment/my-app-deployment my-app=$DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:$GIT_COMMIT'
        }
      }
    }
  }
}
