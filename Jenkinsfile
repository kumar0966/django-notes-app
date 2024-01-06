pipeline {
    agent any

    stages {
        stage('Code') {
            steps {
                echo 'Cloning the code'
                git url:"https://github.com/kumar0966/django-notes-app.git", branch: "kumar_deployment"
            }
        }
        stage('Build') {
            steps {
                echo 'Building the code'
                sh "docker build -t my-note-app ."
            }
        }
        stage('Push to Docker Hub') {
            steps {
                echo 'Pushing the code in dockerhub'
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag my-note-app ${env.dockerHubUser}/my-note-app:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/my-note-app:latest"
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Dreploying the container'
                sh "docker-compose down && docker-compose up"
            }
        }
    }
}
