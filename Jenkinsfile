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
                sh 'docker build . --tag=rahul14m93/devops_capstone_nd'
                sh 'echo "$MY_DOCKER_PASS" | docker login --username rahul14m93 --password-stdin'
                sh 'docker push rahul14m93/devops_capstone_nd:latest'
                
            }
        }
    }

}