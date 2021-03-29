# Rolling deployments

In this project I've demonstrated rolling deployments using Jenkins and a basic web app

The index.html displays a random pokemon everytime you refresh the url

Steps followed to create and deploy this project.

1. Create a user with minimum privileges
2. Create a EC2 instance called Jenkins
3. SSH login into Jenkins and install Jenkins with all the plugins described in the course
4. SSH login into Jenkins EC2 instance and install Docker, kubectl, eksctl and hadolint. Ensure they are available globally under /usr/bin/
5. Connect Jenkins with my GitHub repository
6. Define my rolling deployment pipeline in the Jenkinsfile
7. Push the code to my GitHub repository
8. Wait for the pipeline to start

