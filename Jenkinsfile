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
                withCredentials([usernamePassword(credentialsId: 'docker_hub_credentials', passwordVariable: 'dockerPass', usernameVariable: 'dockerUser')]) {
                    sh "docker tag node-app-test-new ${env.dockerUser}/node-app-test-new:latest"
                    sh "docker login -u ${env.dockerUser} -p ${env.dockerPass}"
                    sh "docker push ${env.dockerUser}/node-app-test-new:latest"
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
