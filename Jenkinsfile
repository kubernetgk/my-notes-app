pipeline {
    agent any
    
    stages {
        stage("Clone Code"){
            steps{
                echo "Cloning the code"
                git url: "https://github.com/kubernetgk/my-notes-app.git", branch: "main"
            }
            
        }
        stage("build"){
              steps{
                echo "Building the image"
                sh "docker build -t my-note-app ."
            }
        }
        stage("push to Docker Hub"){
              steps{
                echo "Pushing the image to docker hub"
                withCredentials([usernamePassword(credentialsId:"DockerSecret",passwordVariable: "dockerpass",usernameVariable: "dockeruser")]) {
                sh "docker tag my-note-app ${env.dockeruser}/my-note-app:latest"
                sh "docker login -u ${env.dockeruser} -p ${env.dockerpass}"
                sh "docker push ${env.dockeruser}/my-note-app:latest"
                }
            }
        }
        stage("Deploy"){
              steps{
                echo "Deploying the container"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
