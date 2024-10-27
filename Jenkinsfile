pipeline {
    agent any

    stages {

                    
        stage ('maven sonar') {
            steps {
                
                sh 'mvn clean'
                sh 'mvn compile'
                sh 'mvn sonar:sonar -Dsonar.login=admin -Dsonar.password=Imen1234567*'
            }
        }
        stage ('maven build') {
            steps {
                sh 'mvn package'
            }
        }

        stage("PUBLISH TO NEXUS") {
            steps {
                sh 'mvn deploy'
            }
        }
        stage('Building docker  image') {
            steps {
                script {

                    sh "  docker build ./ -t imenziedi/tp-foyer:v1 "
                   
                   
                }
            }
        }

        stage('push docker  image'){
            steps{
                script {
                    
                        sh "  docker push  imenziedi/tp-foyer:v1 "
                    }
                }
            }
        
        stage('cleaning image'){
            steps{
                script {
                     
                        sh "  docker rmi imenziedi/tp-foyer:v1"
                    
                }
            }
        }
        stage('run docker compose'){
            steps{
                script {
                     
                        sh " sudo docker compose up"
                    
                }
            }
        }

      
    }
}
