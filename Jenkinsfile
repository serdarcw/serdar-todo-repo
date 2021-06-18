pipeline{
    agent any
    environment {
        MYSQL_DATABASE_HOST = "ecr-docker-jenkins-rds.cbanmzptkrzf.us-east-1.rds.amazonaws.com"
        MYSQL_DATABASE_PASSWORD = "Clarusway"
        MYSQL_DATABASE_USER = "admin"
        MYSQL_DATABASE_DB = "phonebook"
        MYSQL_DATABASE_PORT = 3306
        PATH="/usr/local/bin/:${env.PATH}"
    }
    stages{
        stage("compiled"){
            agent{
                docker{
                    image 'python:alpine'
                }
            }
            steps{
                withEnv(["HOME=${env.WORKSPACE}"]) {
                    sh 'pip install -r requirements.txt'
                    sh 'python -m py_compile src/*.py'
                    stash(name: 'compilation_result', includes: 'src/*.py*')
                }   
            }
        }
        stage('test') {
                agent {
                    docker {
                        image 'python:alpine'
                    }
                }
                steps {
                    withEnv(["HOME=${env.WORKSPACE}"]) {
                        sh 'pip install -r requirements.txt'
                        sh 'python -m pytest -v --junit-xml results.xml src/appTest.py'
                        // bu test dosyasını kullan ve src/appTest.py nin sonucunu pytest modülü aracılığı ile test et ve jnuitin yorumlamasına uygun bir hale getir. ardından resultları result.xml'de topla
                    }
                }
                post {
                    always {
                        junit 'results.xml'
                    }
                }
        }
        stage('build'){
                agent any
                steps{
                    sh "docker build -t 046402772087.dkr.ecr.us-east-1.amazonaws.com/serdarcw/myhandson:latest ."
                }
        }
        stage('push'){
                agent any
                steps{
                    sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 046402772087.dkr.ecr.us-east-1.amazonaws.com"
                    sh "docker push 046402772087.dkr.ecr.us-east-1.amazonaws.com/serdarcw/myhandson:latest"
            }
        }
        stage('docker compose'){
                agent any
                steps{
                    sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 046402772087.dkr.ecr.us-east-1.amazonaws.com"
                    sh "docker-compose up -d"
                }
            }
     }
}

