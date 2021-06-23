pipeline {
    agent { label "master" }
    environment {
        ECR_REGISTRY = "046402772087.dkr.ecr.us-east-1.amazonaws.com"
        APP_REPO_NAME= "clarusway-repo/todo-app"
        PATH="/usr/local/bin/:${env.PATH}"        
    }
    stages {
        stage("running app on Docker"){
            agent{
                docker{
                    image 'node:12-alpine'
                }
            }
            steps{
                withEnv(["HOME=${env.WORKSPACE}"]) {
                    sh 'yarn install --production'
                    sh 'npm install'
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build --force-rm -t "$ECR_REGISTRY/$APP_REPO_NAME" .' 
                sh 'docker image ls'
            }
        }
        stage('Push Image to ECR Repo') {
            steps {
                sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin $ECR_REGISTRY'
                sh 'docker push $ECR_REGISTRY/$APP_REPO_NAME:latest'
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