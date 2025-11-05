@Library("Shared") _
pipeline {
    agent {label 'dhruv'}
    stages {
        stage('Hello!'){
           steps{
               script{
                   hello()
               }
           }
        }
        stage('Git Clone') {
            steps {
                echo "Code Cloning"
                git url: "https://github.com/senjaliyadhruv/task-manager.git", branch:"main"
                echo "Code cloned successfully"
            }
        }
        stage('Build Image'){
            steps{
                echo "Building the code"
                sh 'docker build -t task-manager:latest .'
                echo "Image Builded successfully"
            }
        }
        stage('Run Container'){
            steps{
                echo "Running the code"
                sh 'docker run -dt -p 80:80 task-manager:latest '
                echo "Container started successfully"
            }
        }
        stage('Push image to Docker Hub'){
            steps{
                echo "Image pushing to the DockerHub"
                withCredentials([usernamePassword(
                'credentialsId': "DockerHubCred", 
                passwordVariable:"DockerHubPass", 
                usernameVariable:"DockerHubUser")]){
                sh "docker login -u ${env.DockerHubUser} -p ${env.DockerHubPass}"
                sh "docker image tag task-manager:latest ${env.DockerHubUser}/task-manager:latest"
                sh "docker push ${env.DockerHubUser}/task-manager:latest"
                }
                echo "Image pushed to Docker Hub sucessfully"
            }
        }
    }
}
