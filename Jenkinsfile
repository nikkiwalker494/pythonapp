pipeline {
  agent any
  stages {
    stage('Build Docker Flask Image') {
      steps {
        sh 'docker build -t nwalker494/pyapp:latest:latest:build-${BUILD_NUMBER} .'
      }
    }
    stage('Build Docker nginx Image') {
      steps {
        sh ''' # could us cd nginx here or put the file path at the end of the build
        docker build -t nwalker494/nginxpy-custom:latest:build-${BUILD_NUMBER} ./nginx'''
      }
    }
    stage('Push Docker Image to Registry') {
      steps {
        sh '''
          docker push nwalker494/pyapp:latest}
          docker push nwalker494/pyapp:latest:${BUILD_NUMBER}
          docker push nwalker494/nginxpy-custom:latest
          docker push nwalker494/nginxpy-custom:latest:${BUILD_NUMBER}
          '''
        }
      }
    
    stage('Deploy to Kubernetes') {
      steps {
        sh '''
        cd ./kubernetes
        kubectl apply -f 
        '''
      }
    }
  }
}
