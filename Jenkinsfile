pipeline {
  agent any
  environment {
    DOCKER_REGISTRY = 'nwalker494'
    DOCKER_PASSWORD = "password1"
    DOCKER_USERNAME = "nwalker494"
    // KUBECONFIG = credentials('kubeconfig')
    // KUBE_NAMESPACE = 'your.kubernetes.namespace'
  }
  stages {
    stage('Build Docker Flask Image') {
      steps {
        sh 'docker build -t nwalker494/pyapp:latest:latest:build-${BUILD_NUMBER} .'
      }
    }
    stage('Build Docker nginx Image') {
      steps {
        sh 
        //  could us cd nginx here or put the file path at the end of the build
        'docker build -t nwalker494/nginxpy-custom:latest:build-${BUILD_NUMBER} ./nginx'
      }
    }
    stage('Push Docker Image to Registry') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'nwalker494', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {            
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