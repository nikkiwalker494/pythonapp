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
        sh 'docker build -t nwalker494/nginxpy-custom:latest:${BUILD_NUMBER} .'
      }
    }
    stage('Push Docker Image to Registry') {
      steps {
        // withCredentials([usernamePassword(credentialsId: 'docker-registry-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) 
        {
        //   sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD} ${DOCKER_REGISTRY}"
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