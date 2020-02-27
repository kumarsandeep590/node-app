pipeline {
    agent any
    environment{
    DOCKER_TAG = getDockerTag()
    }
    stages{
        stage('Build Docker Image'){
            steps{
                sh "docker build . -t kumarsandeep590/newapp:${DOCKER_TAG}"
                }
                }
        stage('DockerHub Push'){
            withCredentials([string(credentialsId: 'DockerHub', variable: 'dockerHubPwd')]) {
                sh "docker login -u kumarsandeep590 -p ${dockerHubPwd}"
                sh "docker push kumarsandeep590/nodeapp ${DOCKER_TAG}"
            }
            
            
           }
                }
                }
                def getDockerTag(){
                def tag = sh script: 'git rev-parse HEAD', returnStdout: true
                return tag
                }
