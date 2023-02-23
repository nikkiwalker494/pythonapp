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
        sh 'docker build -t eu.gcr.io/lbg-cloud-incubation/pyapp:latest -t eu.gcr.io/lbg-cloud-incubation/pyapp:build-${BUILD_NUMBER} .'
      }
    }
    stage('Build Docker nginx Image') {
      steps {
        sh ''' # could us cd nginx here or put the file path at the end of the build
        docker build -t eu.gcr.io/lbg-cloud-incubation/nginxpy-custom:latest -t eu.gcr.io/lbg-cloud-incubation/nginxpy-custom:build-${BUILD_NUMBER} ./nginx'''
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
          docker push eu.gcr.io/lbg-cloud-incubation/pyapp:latest 
          docker push eu.gcr.io/lbg-cloud-incubation/pyapp:build-${BUILD_NUMBER}
          docker push eu.gcr.io/lbg-cloud-incubation/nginxpy-custom:latest
          docker push eu.gcr.io/lbg-cloud-incubation/nginxpy-custom:build-${BUILD_NUMBER}
          '''
        }
      }    
    
    stage('Deploy to Kubernetes') {
      steps {
        script {
            if ("${GIT_BRANCH}" == 'origin/main') {
                        sh '''
                            cd ./kubernetes
                            
                            sed -e 's,{{ns}},production,g;' application.yml | kubectl apply -f -
                            sed -e 's,{{ns}},production,g;' service.yml | kubectl apply -f -
                            kubectl rollout restart deployment --namespace=production py-app
                            kubectl rollout restart deployment --namespace=production nginxpy

                            '''
        } else if ("${GIT_BRANCH}" == 'origin/development') {
                            sh '''
                            cd ./kubernetes
                            sed -e 's,{{ns}},development,g;' application.yml | kubectl apply -f -
                            sed -e 's,{{ns}},development,g;' service.yml | kubectl apply -f -
                            kubectl rollout restart deployment --namespace=development py-app
                            kubectl rollout restart deployment --namespace=development nginxpy

                            '''
      }
    }
  }
    }
  }
}