pipeline {
    agent any
    stages {
        stage("Clone"){
            steps {
                echo "Cloning code"
                git url:"https://github.com/spelluri/react-app.git",branch: "main"
                echo "Code cloned correctly"
            }
        }
        stage("Build"){
            steps{
                echo "docker image build"
                sh "docker build -t my-react-app ."
                echo "docker image build done correctly"
            }
        }
        stage("Login and Push"){
            steps {
                echo "Login to Dockerhub and Push imahe to dockerhub"
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerhubPass",usernameVariable:"dockerhubUser")]){
                    echo "dockerhub login"
                    sh "docker login -u ${env.dockerhubUser} -p ${env.dockerhubPass}"
                    echo "dockerhub login done correctly"
                    echo "taging docker image"
                    sh "docker tag my-react-app ${env.dockerhubUser}/my-react-app:${BUILD_NUMBER}"
                    echo "push docker image to dockerhub"
                    sh "docker push ${env.dockerhubUser}/my-react-app:${BUILD_NUMBER} "
                }
            }
        }
        stage("deploy") {
            steps {
                echo "deploying docker image"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
