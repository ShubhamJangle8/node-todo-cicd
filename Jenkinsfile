pipeline {
    agent any
    stages{
        stage("Clone Code"){
            steps{
                git url: "https://github.com/ShubhamJangle8/node-todo-cicd.git", branch: "master"
            }
        }
        stage("Build and Test"){
            steps{
                sh "docker build . -t node-app-test-new"
            }
        }
        stage("Push to Docker Hub"){
            steps{
                withCredentials([usernamePassword(credentialsId: 'docker_hub_credentials', passwordVariable: 'docker-pass', usernameVariable: 'docker-user')]) {
                    sh "docker tag node-app-test-new ${env.docker-user}/node-app-test-new:latest"
                    sh "docker login -u ${env.docker-user} -p ${env.docker-pass}"
                    sh "docker push ${env.docker-user}/node-app-test-new:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
