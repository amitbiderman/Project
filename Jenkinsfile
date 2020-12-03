pipeline {
    agent any
    environment{
        DOCKER_TAG = getDockerTag()
    }
    stages{
        stage('Build Docker Image'){
            steps{
                sh "docker build . -t ${DOCKER_TAG}"
            }
        }
        stage('Dockerhub Push'){
            steps{
                withCredentials([string(credentialsId: 'docker-hub', variable: 'dockerHubPwd')]) {
                    sh "docker login -u amitbiderman -p ${dockerHubPwd}"
                    sh "docker push amitbiderman/Project ${DOCKER_TAG}"
                }
            }
        }
// Utility method - gets latest commit ID from git
def getDockerTag(){
    def tag  = sh script: 'git rev-parse HEAD', returnStdout: true
    return tag
}
