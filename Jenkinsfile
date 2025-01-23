pipeline {
    agent { label 'dev-server' }
    
    stages {
        stage("Clone") {
            steps {
                git url: "https://github.com/LondheShubham153/node-todo-cicd.git", branch: "master"
            }
        }
        stage("Code Build & Test") {
            steps {
                sh "docker build -t node-app ."
            }
        }
        stage("Push to DockerHub") {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "DockerHubCred",
                    usernameVariable: "DockerHubUser",
                    passwordVariable: "DockerHubPass"
                )]) {
                    sh "docker login -u ${DockerHubUser} -p ${DockerHubPass}"
                    sh "docker image tag node-app:latest ${DockerHubUser}/node-app:latest"
                    sh "docker push ${DockerHubUser}/node-app:latest"
                }
            }
        }
        stage("Deploy") {
            steps {
                sh "docker compose down && docker compose up -d --build"
            }
        }
    }
}
