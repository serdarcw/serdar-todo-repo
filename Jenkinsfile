pipeline {
    agent { label "master" }
    environment {
        ECR_REGISTRY = "046402772087.dkr.ecr.us-east-1.amazonaws.com"
        APP_REPO_NAME= "clarusway/serdar"
    }
    stages {
        stage('Build Docker Image') {
            steps {
                sh 'docker build --force-rm -t "$ECR_REGISTRY/$APP_REPO_NAME:latest" .'
                sh 'docker image ls'
            }
        }
        stage('Push Image to ECR Repo') {
            steps {
		script {
	          docker.withRegistry(
		     '046402772087.dkr.ecr.us-east-1.amazonaws.com',
                     'ecr:us-east-1:my_aws_credentials') {
                     def myImage = docker.build('/clarusway/serdar')
		     myImage.pusg('latest')	
		   }
                }
            }
        }
     } 
    post {
        always {
            echo 'Deleting all local images'
            sh 'docker image prune -af'
        }
    }
}
