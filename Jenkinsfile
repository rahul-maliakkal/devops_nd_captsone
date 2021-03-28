pipeline {
     agent any
     stages {
        stage('Build') {
              steps {
                  sh 'echo Building......'
              }
         }

        stage("Lint Dockerfile") {
			steps {
      		              sh 'sudo /bin/hadolint Dockerfile'
			}
		} 
        
        stage('Build and Upload docker Image')
		{
			steps{
				sh 'docker build . --tag=rahul14m93/capstone'
				
					sh 'docker push rahul14m93/capstone:latest'
				}
			}
		}


  }
}