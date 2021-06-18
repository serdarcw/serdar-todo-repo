pipeline {
    agent { label "master" }
    environment {
        ECR_REGISTRY = "046402772087.dkr.ecr.us-east-1.amazonaws.com"
        APP_REPO_NAME= "clarusway/to-do-app"
        PATH="/usr/local/bin/:${env.PATH}"
    }
    stages {
        stage("compile"){
            agent{
                docker{
                    image 'node:12-alpine'
                }
            }
            steps{
                withEnv(["HOME=${env.WORKSPACE}"]) {
                    sh 'yarn install'
                    sh 'npm install'
                }   
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build --force-rm -t "$ECR_REGISTRY/$APP_REPO_NAME:latest" .'
                sh 'docker image ls'
            }
        }
        stage('Push Image to ECR Repo') {
            steps {
                sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin "$ECR_REGISTRY"'
                sh 'docker push "$ECR_REGISTRY/$APP_REPO_NAME:latest"'
            }
        }
    }
    post {
        success {
            echo 'You did it. You are gonna be a good Devops'
        }
        always {
            echo 'Deleting all local images'
            sh 'docker image prune -af'
        }
    }
}

