pipeline {
    agent any
    
    stages{
        stage("Code"){
            steps{
                git url: "https://github.com/manishcs3/node-todo-cicd.git", branch: "master"
            }
        }
        stage("Build & Test"){
            steps{
                sh "docker build . -t node-app-test-new"
            }
        }
        stage("Push to DockerHub"){
            steps{
                withCredentials([usernamePassword(credentialsId:"jenkinsDocker",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker tag node-app-test-new ${env.dockerHubUser}/node-app-test-new:v1"
                    sh "docker push ${env.dockerHubUser}/node-app-test-new:v1" 
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker compose down && docker compose up -d --build"
            }
        }
    }
}
