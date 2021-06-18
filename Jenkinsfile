pipeline {
    agent { label "master" }
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
