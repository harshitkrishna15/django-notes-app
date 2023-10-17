pipeline {
    agent any
    
    stages{
        stage("clone code"){
            steps{
                echo "cloning the code"
                git url: "https://github.com/harshitkrishna15/django-notes-app.git",branch: "main"
            }
        }
        stage("build"){
             steps{
                echo "building the image"
               sh "docker build -t my-note-app ."
            }
        }
        stage("push to docker hub"){
             steps{
                echo "pushing the image to docker hub"
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerhubpass",usernameVariable:"dockerhubuser")]){
                sh "docker tag my-note-app ${env.dockerhubuser}/my-note-app:latest"
                sh "docker login -u ${env.dockerhubuser} -p ${env.dockerhubpass}"
                sh "docker push ${env.dockerhubuser}/my-note-app:latest"
                 }
            }
        }
        stage("deploy"){
             steps{
                echo "deploying the container"
                // sh "docker run -d -p 8000:8000 15092002/my-note-app:latest"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
