pipeline {
     agent any
     stages {
        stage('Build') {
              steps {
                  sh 'echo Building........'
              }
         }

        stage("Lint Dockerfile") {
			steps {
      		              sh 'sudo /bin/hadolint Dockerfile'
			}
		} 
        
        


  }
}