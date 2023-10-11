pipeline {
    agent any
    
    stages{
        stage("Clone Code"){
            steps {
                echo "Cloning the Code"
                git url:"https://github.com/lakshaygautam1992/django-notes-app.git", branch: "main"
            }
        }
        stage("Build the image"){
            steps {
                echo "Building the image"
                sh "docker build -t my-note-app ."
            }
        }
        stage("Push to Docker Hub"){
            steps {
                echo "Pushing the image"
                withCredentials([usernamePassword(credentialsId:"docker_Hub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag my-note-app ${env.dockerHubUser}/my-note-app:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/my-note-app:latest"
                }
            }
        }
        stage("Deploy"){
            steps {
                echo "Deploying the Container"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
