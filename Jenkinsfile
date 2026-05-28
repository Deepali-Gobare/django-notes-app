pipeline{
    agent{label "vinod"}
    stages{
        stage("code"){
           steps{
               echo "This is cloning the code"
               git url:"https://github.com/Deepali-Gobare/django-notes-app.git" , branch:"main"
               echo "success"
           } 
        }
        stage("Build"){
            steps{
                 echo "This is Building the code"
                 sh "whoami"
                 sh "docker build -t notes-app:latest ."
            }
        }
        stage("Push to DockerHub"){
            steps{
                 echo "This is pushing to the dockerhub"
                  withCredentials([usernamePassword(
                    credentialsId:"dockerHubCred",
                    usernameVariable:"dockerHubUser", 
                    passwordVariable:"dockerHubPass")]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker image tag notes-app:latest ${env.dockerHubUser}/notes-app:latest"
                sh "docker push ${env.dockerHubUser}/notes-app:latest"
                    }
            }
        }
        stage("Deploy"){
           steps{
                echo "This is deploying the code"
              
           } 
        }
    }
}