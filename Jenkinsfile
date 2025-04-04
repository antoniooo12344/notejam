pipeline {
    agent any
    environment {
        PROJECT_ID = 'laba1clttech123'
                CLUSTER_NAME = 'autopilot-cluster-1'
                LOCATION = 'europe-west6-c'
                CREDENTIALS_ID = 'laba1clttech123'
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build image') {
            steps {
                script {
                    app = docker.build("laba3clttec/pipeline:${env.BUILD_ID}")
                    }
            }
        }
        
        stage('Push image') {
            steps {
                script {
                    withCredentials( \
                                 [string(credentialsId: 'dockerhub',\
                                 variable: 'dockerhub')]) {
                        powershell "docker login -u laba3clttec -p '${dockerhub}'"
                    }
                    app.push("${env.BUILD_ID}")
                 }
                                 
            }
        }
    
        stage('Deploy to K8s') {
            steps{
                echo "Deployment started ..."
                powershell '''
                    $buildId = $env:BUILD_ID
                    (Get-Content deployment.yaml) -replace 'pipeline:latest', "pipeline:$buildId" | Set-Content deployment.yaml
                '''
                step([$class: 'KubernetesEngineBuilder', \
                  projectId: env.PROJECT_ID, \
                  clusterName: env.CLUSTER_NAME, \
                  location: env.LOCATION, \
                  manifestPattern: 'deployment.yaml', \
                  credentialsId: env.CREDENTIALS_ID, \
                  verifyDeployments: true])
                }
            }
        }    
}
