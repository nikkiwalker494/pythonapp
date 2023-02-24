pipeline {
    agent any
    stages {
        stage('Build Flask App') {
            steps {
                script {
			        if ("${GIT_BRANCH}" == 'origin/main') {
				        sh '''
                        echo "Skipping due to main branch"
				        '''
			        } else if ("${GIT_BRANCH}" == 'origin/development') {
				        sh '''
				        docker build -t nwalker494/pyapp:latest -t nwalker494/pyapp:build-${BUILD_NUMBER} .
				        '''
			        } 
		        }
           }
        }
        stage('Build Custom NGINX') {
            steps {
                script {
			        if ("${GIT_BRANCH}" == 'origin/main') {
				        sh '''
                        echo "Skipping due to main branch"
				        '''
			        } else if ("${GIT_BRANCH}" == 'origin/development') {
				        sh '''
                        cd ./nginx # could us cd nginx here or put the file path at the end of the build ./nginx
                        docker build -t nwalker494/nginxpy-custom:latest -t nwalker494/nginxpy-custom:build-${BUILD_NUMBER} .
				        '''
			        } 
                }
           }
        }
        stage('Push Images') {
            steps {
                script {
			        if ("${GIT_BRANCH}" == 'origin/main') {
				        sh '''
                        echo "Skipping due to main branch"
				        '''
			        } else if ("${GIT_BRANCH}" == 'origin/development') {
				        sh '''
                        docker push eu.gcr.io/lbg-cloud-incubation/pyapp:latest 
                        docker push eu.gcr.io/lbg-cloud-incubation/pyapp:build-${BUILD_NUMBER}
                        docker push eu.gcr.io/lbg-cloud-incubation/nginxpy-custom:latest
                        docker push eu.gcr.io/lbg-cloud-incubation/nginxpy-custom:build-${BUILD_NUMBER}
				        '''
			        } 
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
			        if ("${GIT_BRANCH}" == 'origin/main') {
				        sh '''
                        cd ./kubernetes
				        sed -e 's,{{ns}},production,g;' namespace.yml | kubectl apply -f -
                        sed -e 's,{{ns}},production,g;' application.yml | kubectl apply -f -
                        sed -e 's,{{ns}},production,g;' service.yml | kubectl apply -f -
                        kubectl rollout restart deployment --namespace=production py-app
                        kubectl rollout restart deployment --namespace=production nginxpy
				        '''
			        } else if ("${GIT_BRANCH}" == 'origin/development') {
				        sh '''
				        cd ./kubernetes
				        sed -e 's,{{ns}},development,g;' namespace.yml | kubectl apply -f -
                        sed -e 's,{{ns}},development,g;' application.yml | kubectl apply -f -
                        sed -e 's,{{ns}},development,g;' service.yml | kubectl apply -f -
                        kubectl rollout restart deployment --namespace=development py-app
                        kubectl rollout restart deployment --namespace=development nginxpy
				        '''
			        } 
		        }
                
            }
        }
        stage('Clean Up') {
            steps {
                sh 'docker system prune -f'
            } 
        }
    }
}