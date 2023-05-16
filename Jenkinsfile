pipeline{
    agent {label "dev"}
    stages{
        stage("Clone Code"){
            steps{
                git url: "https://github.com/neelsoni26/react_django_demo_app", branch: "main"
            }
        }
        stage("Build and Test") {
            steps{
                sh "docker build . -t django-todo-app"
            }
        }
        stage("Login and Push image")
        {
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerhubPassword",usernameVariable:"dockerhubUsername")]){
                    sh """
                    #!/bin/sh
                    docker tag django-todo-app ${env.dockerhubUsername}/django-todo-app:latest
                    docker login -u ${env.dockerhubUsername} -p ${env.dockerhubPassword}
                    docker push ${env.dockerhubUsername}/django-todo-app:latest
                    """
                }
            }
        }
        stage("Deploy") {
            steps{
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}