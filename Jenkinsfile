pipeline {
agent any
    environment {
   	 PROJECT_ID = 'docker'
            	CLUSTER_NAME = 'jenkins'
            	LOCATION = 'us-central-1a'
            	CREDENTIALS_ID = 'kubernetes'
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
   			 	app = docker.build("zetzo/pipeline:{env.BUILD_ID}")
   		  	}
   						 	 
   	 	}
    	}
   
    	stage('Deploy to K8s') {
    steps {
        echo "Deployment started ..."
        sh 'ls -ltr'
        sh 'pwd'
        sh "sed -i 's/pipeline:latest/pipeline:${BUILD_NUMBER}/g' deployment.yaml"
        kubernetesDeploy(
            configs: 'deployment.yaml',
            kubeconfigId: env.CREDENTIALS_ID,
            enableConfigSubstitution: true
        )
    }
}

   	 }    
}
