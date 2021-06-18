pipeline{
    agent any
    environment {
        MYSQL_DATABASE_HOST = "jenkins-ecr-hands-on.cbanmzptkrzf.us-east-1.rds.amazonaws.com"
        MYSQL_DATABASE_PASSWORD = "Clarusway"
        MYSQL_DATABASE_USER = "admin"
        MYSQL_DATABASE_DB = "phonebook"
        MYSQL_DATABASE_PORT = 3306
    }
    stages{
        stage("compile"){
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
                    sh 'nohup python app.py &'
                    sh 'python -m pytest -v --junit-xml results.xml src/appTest.py'
                }
            }
            post {
                always {
                    junit 'results.xml'
                }
            }
        }
    }
}