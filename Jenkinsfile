pipeline {
    agent any
    environment {
        AWS_REGION = 'ap-south-1'
        AWS_CREDENTIALS = 'eks_access'
        DOCKER_HUB_CREDENTIALS = 'dockerhub'
		CLUSTER_NAME = 'devops-nd-capstone'
    }

    stages {
        stage('Echo Test') {
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
                withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: "${DOCKER_HUB_CREDENTIALS}", usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]){
                    sh '''
                        docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD
                        docker push rahul14m93/devops_capstone_nd:latest
                    '''
                }
            }

            
        }

        stage('Kubernetes cluster') {
			steps {
				withAWS(region:"${AWS_REGION}", credentials:"${AWS_CREDENTIALS}") {
					sh '''
						if aws cloudformation describe-stacks --stack-name eksctl-${CLUSTER_NAME}-cluster; then
							echo 'Stack already exists'
						else
							echo 'Stack is being created'
							eksctl create cluster \
                            --name ${CLUSTER_NAME} \
                            --version 1.19 \
                            --region ${AWS_REGION} \
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
                    withAWS(region:"${AWS_REGION}", credentials:"${AWS_CREDENTIALS}") {
                        sh '''
                            aws eks --region ${AWS_REGION} update-kubeconfig --name ${CLUSTER_NAME}
                            kubectl apply -f deployment.yml
                            kubectl get nodes
                            kubectl get deployment
                            kubectl get pod -o wide
                            kubectl apply -f services.yml
                            kubectl get services
                        '''
                  }
              }
         }
    }

}