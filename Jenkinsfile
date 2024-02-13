pipeline{
    
    agent { label "dev-server-label" }
    
    stages{
        stage("code clone"){
            steps{
                git url: "https://github.com/yogik17/node-todo-cicd.git/", branch: "master"
                echo "Code clonning steps is completed successfully"
            }
        } 
        
        stage("build & test"){
            steps{
                sh "docker build -t node-app-test-new ."
                echo "Image build and test pass successfully"
            }
        }
        
        stage("scan image"){
            steps{
                echo "after build image scan image using trivy(devsecops)"
            }
        }
        
        stage("image push"){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker tag node-app-test-new:latest ${env.dockerHubUser}/node-app-test-new:latest"
                sh "docker push ${env.dockerHubUser}/node-app-test-new:latest"
                echo "Image push to dockerhub successfully"
                }
            }
        }
        
        stage("Deploy"){
            steps{
                sh "docker-compose down && docker-compose up -d"
                echo "Deploy complete code on server successfully"
            }
        }
    }
}
