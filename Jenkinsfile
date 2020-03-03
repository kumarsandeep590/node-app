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
        stage('Deploy Production'){
            steps{
               git url: 'https://github.com/viglesiasce/sample-app'
                step([$class: 'KubernetesEngineBuilder', 
                        projectId: "elevated-range-270006",
                        clusterName: "production",
                        zone: "us-central1-f",
                        manifestPattern: 'k8s/production/',
                        credentialsId: "gke-service-account",
                        verifyDeployments: true])
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
