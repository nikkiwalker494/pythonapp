pipeline {
  agent any
  stages {
    stage('Build Docker Flask Image') {
    //   steps {
    //     sh 'docker build -t nwalker494/pyapp:latest -t nwalker494/pyapp:build-${BUILD_NUMBER} .'
    //   }
    // }
    // stage('Build Docker nginx Image') {
    //   steps {
    //     sh ''' # could us cd nginx here or put the file path at the end of the build
    //     docker build -t nwalker494/nginxpy-custom:latest -t nwalker494/nginxpy-custom:build-${BUILD_NUMBER} ./nginx'''
    //   }
    // }
      steps {
        sh 'docker build -t eu.gcr.io/lbg-cloud-incubation/pyapp:latest -t nwalker494/pyapp:build-${BUILD_NUMBER} .'
      }
    }
    stage('Build Docker nginx Image') {
      steps {
        sh ''' # could us cd nginx here or put the file path at the end of the build
        docker build -t eu.gcr.io/lbg-cloud-incubation/nginxpy-custom:latest -t nwalker494/nginxpy-custom:build-${BUILD_NUMBER} ./nginx'''
      }
    }    
    // stage('Push Docker Image to Registry') {
    //   steps {
    //     sh '''
    //       docker push nwalker494/pyapp:latest
    //       docker push nwalker494/pyapp:build-${BUILD_NUMBER}
    //       docker push nwalker494/nginxpy-custom:latest
    //       docker push nwalker494/nginxpy-custom:build-${BUILD_NUMBER}
    //       '''
    //     }
    //   }

    stage('Push Docker Image to Registry') {
      steps {
        sh '''
          docker push eu.gcr.io/lbg-cloud-incubation/nwalker494/pyapp:latest
          docker push eu.gcr.io/lbg-cloud-incubation/nwalker494/pyapp:build-${BUILD_NUMBER}
          docker push eu.gcr.io/lbg-cloud-incubation/nwalker494/nginxpy-custom:latest
          docker push eu.gcr.io/lbg-cloud-incubation/nwalker494/nginxpy-custom:build-${BUILD_NUMBER}
          '''
        }
      }    
    
    stage('Deploy to Kubernetes') {
      steps {
        sh '''
        cd ./kubernetes
        kubectl apply -f .
        '''
      }
    }
  }
}
