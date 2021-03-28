pipeline {
    agent any
    stages {
        stage('Build') {
                steps {
                    sh 'echo Testing .....'
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
                sh 'docker build . --tag=rahul14m93/capstone'
                sh 'docker push rahul14m93/capstone:latest'
                
            }
        }
    }

}