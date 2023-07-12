pipeline {
    agent any 
    
    stages{
        stage("Clone Code From Git Hub"){
            steps {
                echo "Cloning the code"
                git url:"https://github.com/anupam7860/django-todo.git", branch: "develop"
            }
        }
        stage("Building Docker image"){
            steps {
                echo "Building the image"
                sh "docker build -t myapp ."
            }
        }
        stage("Pushing Image to Docker Hub"){
            steps {
                echo "Pushing the image to docker hub"
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerhubPass",usernameVariable:"dockerhubUser")]){
                sh "docker tag myapp ${env.dockerhubUser}/myapp:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/myapp:latest"
                }
            }
        }
        stage("Deploy to K8 Cluster"){
            steps {
               script{kubernetesDeploy (configs: 'deploymentservice.yaml',kubeconfigId: 'k')
                   
               }
                
            }
        }
    }
}