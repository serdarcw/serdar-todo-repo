pipeline {
    agent { label "master" }
    environment {
        HOME = '.'
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
                    sh 'yarn install --production'
                    sh 'node ./src/index.js'
                }   
            }
        }
    }        
}
