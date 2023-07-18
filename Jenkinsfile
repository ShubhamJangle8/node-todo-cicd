pipeline {
    agent any
    stages{
        stage("Clone Code"){
            steps{
                echo 'clone code'
                git url: "https://github.com/ShubhamJangle8/node-todo-cicd.git", branch: "master"
            }
        }
        stage("Build and Test"){
            steps{
                echo 'build and test'
                sh "docker build . -t node-app-test-new"
            }
        }
        stage("Push to Docker Hub"){
            steps{
                echo 'push'
                withCredentials([usernamePassword(credentialsId: 'docker_hub_credentials', passwordVariable: 'docker-pass', usernameVariable: 'docker-user')]) {
                    sh "docker tag node-app-test-new ${env.docker-user}/node-app-test-new:latest"
                    sh "docker login -u ${env.docker-user} -p ${env.docker-pass}"
                    sh "docker push ${env.docker-user}/node-app-test-new:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                echo 'deploy'
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
