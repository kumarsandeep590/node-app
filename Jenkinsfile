pipeline {
    agent any
    environment{
        DOCKER_TAG = getDockerTag()
    }
    stages{
        stage('Build Docker Image'){
            steps{
                sh "docker build . -t kumarsandeep590/node-app:${DOCKER_TAG}"
            }
        }
        stage('DockerHub Push'){
            steps{
                withCredentials([string(credentialsId: 'DockerHub', variable: 'DockerHub')]) {
                    sh "docker login -u kumarsandeep590 -p ${DockerHub}"
                    sh "docker push kumarsandeep590/node-app:${DOCKER_TAG}"
                }
            }
        }
        stage('Deploy to k8s'){
            steps{
                sh "chmod +x changeTag.sh"
                sh "./changeTag.sh ${DOCKER_TAG}"
                sshagent(['kops-machine']) {
                    sh "scp -o StrictHostKeyChecking=no services.yml node-app-pod.yml root@15.206.68.45:/root/"
                    script{
                        try{
                            sh "ssh root@15.206.68.45 kubectl apply -f ."
                        }catch(error){
                            sh "ssh root@15.206.68.45 kubectl create -f ."
                        }
                    }
                }
            }
        }
    }
}
def getDockerTag(){
    def tag = sh script: 'git rev-parse HEAD', returnStdout: true
    return tag
}

