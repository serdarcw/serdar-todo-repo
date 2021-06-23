pipeline {
    agent { label "master" }
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
                sh 'docker build --force-rm -t "046402772087.dkr.ecr.us-east-1.amazonaws.com/clarusway/to-do-app" .' 
                sh 'docker image ls'
            }
        }
        stage('Push Image to ECR Repo') {
            steps {
                sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 46402772087.dkr.ecr.us-east-1.amazonaws.com'
                sh 'docker push 046402772087.dkr.ecr.us-east-1.amazonaws.com/clarusway/to-do-app:latest'
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