pipeline {
    agent any
    environment{
        DOCKER_TAG = getDockerTag()
    }
    stages{
        stage('Build Docker Image'){
            steps{
                sh "docker build . -t amitbiderman/project:${DOCKER_TAG} "
            }
        }
        stage('Dockerhub Push'){
            steps{
                withCredentials([string(credentialsId: 'docker-hub', variable: 'dockerHubPwd')]) {
                    sh "docker login -u amitbiderman -p ${dockerHubPwd}"
                    sh "docker push amitbiderman/project:${DOCKER_TAG}"
                    sshagent(['project-machine']) {
                    sh "scp -o StrictHostKeyChecking=no services.yml node-app-pod.yml ec2-user@3.127.38.161:/home/ec2-user/"
                    script{
                        try{
                            sh "ssh ec2-user@3.127.38.161 kubectl apply -f"
                        }catch(error){
                            sh "ssh ec2-user@3.127.38.161 kubectl create -f"
                        }
                    }
                }
                }
            }
        }
    }
}


// Utility method - gets latest commit ID from git
def getDockerTag(){
    def tag  = sh script: 'git rev-parse HEAD', returnStdout: true
    return tag
}