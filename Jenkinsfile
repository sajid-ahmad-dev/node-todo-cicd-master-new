pipeline{
    agent { label 'dev-agent' }
    stages{
        stage("Code Clone"){
            steps{
                echo "Code Clone Stage"
                git url: "https://github.com/sajid-ahmad-dev/node-todo-cicd-master-new.git", branch: "main"
            }
        }
        stage("Code Build & Test"){
            steps{
                echo "Code Build Stage"
                sh "docker build . -t sajidahmadlrn/node-todo-cicd:latest"
            }
        }
        stage("Push To DockerHub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId:"dockerhub",
                    usernameVariable:"dockerHubUser", 
                    passwordVariable:"dockerHubPass")]){
                sh 'echo $dockerHubPass | docker login -u $dockerHubUser --password-stdin'
                
                sh "docker push $dockerHubUser/node-todo-cicd:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker-compose down && docker-compose up -d --no-deps --build web"
            }
        }
    }
}
