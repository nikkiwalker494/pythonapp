pipeline {
  agent any
  environment {
    DOCKER_REGISTRY = 'nwalker494'
    // KUBECONFIG = credentials('kubeconfig')
    // KUBE_NAMESPACE = 'your.kubernetes.namespace'
  }
  stages {
    stage('Build Docker Image') {
      steps {
        sh 'docker build -t nwalker494/nginxpy-custom:latest .'
      }
    }
    stage('Push Docker Image to Registry') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'docker-registry-credentials', usernameVariable: 'nwalker494', passwordVariable: 'password1')]) {            
          sh "docker push nwalker494/nginxpy-custom:latest:${BUILD_NUMBER}"
        }
      }
    }
    // stage('Deploy to Kubernetes') {
    //   steps {
    //     sh "kubectl apply -f kubernetes/deployment.yaml --namespace=${KUBE_NAMESPACE} --kubeconfig=${KUBECONFIG}"
    //   }
    }
  }
// }