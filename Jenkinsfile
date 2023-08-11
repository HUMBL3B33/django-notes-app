pipeline {
    agent any
    
    stages {
        stage("Code"){
            steps {
                echo "Cleaning the code"
                git url:"https://github.com/HUMBL3B33/django-notes-app.git" , branch:"main"
            }
        }
        stage("Build"){
            steps {
                echo "Building the image"
                sh "docker build -t my-notes-app ."
            }
        }
        stage("Push to Docker Hub"){
            steps{
                echo "Pushing the image to docker hub"
                withCredentials([usernamePassword(credentialsId:"docker-hub",passwordVariable:"dockerhubPass",usernameVariable:"dockerhubUser")]){
                sh "docker tag my-notes-app:latest ${env.dockerhubUser}/my-notes-app:latest"
                sh "docker login -u ${env.dockerhubUser} -p ${env.dockerhubPass}"
                sh "docker push ${env.dockerhubUser}/my-notes-app:latest"
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
