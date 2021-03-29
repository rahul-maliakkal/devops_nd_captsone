CLUSTER_NAME=devops-nd-captsone

pipeline {
    agent any
    stages {
        stage('Build') {
                steps {
                    sh 'echo Testing ....'
                }
            }

        stage("Lint Dockerfile") {
            steps {
                sh '/bin/hadolint Dockerfile'
            }
        } 

        stage('Build and Upload docker Image')
        {
            steps{
                sh 'docker build . --tag=rahul14m93/devops_capstone_nd'
                withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]){
                    sh '''
                        docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD
                        docker push rahul14m93/devops_capstone_nd:latest
                    '''
                }
            }

            
        }

        stage('Kubernetes cluster') {
			steps {
				withAWS(region:'ap-south-1', credentials:'eks_access') {
					sh '''
						if aws cloudformation describe-stacks --stack-name eksctl-${CLUSTER_NAME}-cluster; then
							echo 'Stack already exists'
						else
							echo 'Stack is being created'
							eksctl create cluster \
                            --name devops-nd-captsone \
                            --version 1.19 \
                            --region ap-south-1 \
                            --nodegroup-name linux-nodes \
                            --nodes 2 \
                            --nodes-min 1 \
                            --nodes-max 4
						fi
					'''
				}
			}
		}

         stage('Deploy to AWS Kubernetes Cluster') {
                  steps {
                    withAWS(region:'ap-south-1', credentials:'eks_access') {
                        sh "aws eks --region ap-south-1 update-kubeconfig --name devops-nd-captsone"
                        sh "kubectl apply -f deployment.yml"
                        sh "kubectl get nodes"
                        sh "kubectl get deployment"
                        sh "kubectl get pod -o wide"
                        sh "kubectl apply -f services.yml"
                        sh "kubectl get services"
                  }
              }
         }
    }

}