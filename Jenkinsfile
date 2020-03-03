pipeline {
    agent any
    environment {
        PROJECT_ID = 'elevated-range-270006'
        CLUSTER_NAME = 'kubectl'
        LOCATION = 'us-east1-c'
        CREDENTIALS_ID = 'My First Project'
    }
    stages {
        stage("Checkout code") {
            steps {
                checkout scm
            }
        }
        stage("Build image") {
            steps {
                script {
                    myapp = docker.build("kumarsandeep590/node-app:${env.BUILD_ID}")
                }
            }
        }
        stage('DockerHub Push'){
            steps{
                withCredentials([string(credentialsId: 'DockerHub', variable: 'DockerHub')]) {
                    sh "docker login -u kumarsandeep590 -p ${DockerHub}"
                    sh "docker push kumarsandeep590/node-app:${env.BUILD_ID}"
                }
            }
        }
        stage('Deploy to GKE') {
            steps{
                sh "sed -i 's/node-app:latest/node-app:${env.BUILD_ID}/g' services.yml"
                step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'deployment.yaml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
            }
        }
    }    
}
