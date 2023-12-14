pipeline {
    agent any 
    
    stages{
        stage("Clone Code"){
            steps {
                echo "Cloning the code"
                git url:"https://github.com/abhibham/django-notes-app.git", branch: "main"
            }
        }
        stage("Build"){
            steps {
                echo "Building the image"
                sh "docker build -t my-note-app ."
            }
        }
        stage("Push to Docker Hub"){
            steps {
                echo "Pushing the image to docker hub"
                withCredentials([usernamePassword(credentialsId:"Dockerhub",passwordVariable:"DockerhubPass",usernameVariable:"DockerhubUser")]){
                sh "docker tag my-note-app ${env.DockerhubUser}/my-note-app:latest"
                sh "docker login -u ${env.DockerhubUser} -p ${env.DockerhubPass}"
                sh "docker push ${env.DockerhubUser}/my-note-app:latest"
                }
            }
        }
        stage("Deploy"){
            steps {
                echo "Deploying the container"
                sh "docker-compose down && docker-compose up -d"
                
            }
        }
    }
}
