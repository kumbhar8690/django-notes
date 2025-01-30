@Library("SharedLib") _ 
pipeline {
    agent { label "Agent1"}

    stages {
        stage('Pull from SourceCode') {
            steps {
                script{
                echo 'This is Code Stage'
                clone("https://github.com/kumbhar8690/django-notes.git", "main")
                echo 'Git Cloning is successfull'
                }
            }
        }
        stage('BuildImage and Test') {
            steps {
                script{
                echo 'This is Build Stage'
                sh "whoami"
                docker_build("notes-app", "latest", "glaxm")
                echo "Build is Successfull"
                }
            }
        }
        stage('Push to Repo') {
            steps {
                echo 'This is pushing image to DockerHub Stage'
                withCredentials([usernamePassword(
                    'credentialsId':"DockerHubkey",
                    usernameVariable:"dockerHubUser", 
                    passwordVariable:"dockerHubPass")]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass} "
                sh "docker image tag notes-app:latest ${env.dockerHubUser}/notes-app:latest"
                sh "docker push ${env.dockerHubUser}/notes-app:latest"
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'This is Deploy Stage'
                sh "docker compose up -d"
                echo "Deployment is successfull"
            }
        }
    }
}
