pipeline {
    agent any
    environment {
        registry = "nagui69/kaddem"
        registryCredential = 'dockerhub'
        dockerImage = ''
    
    }

    stages {

                    
        stage ('maven sonar') {
            steps {
                
                sh 'mvn clean'
                sh 'mvn compile'
                sh 'mvn sonar:sonar -Dsonar.login=admin -Dsonar.password=admin1'
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

                    sh " docker build ./ -t nagui69/kaddem:abdelhak "
                   
                   
                }
            }
        }

        stage('push docker  image'){
            steps{
                script {
                     docker.withRegistry('', registryCredential) {
                        sh " docker push nagui69/kaddem:abdelhak "
                    }
                }
            }
        }
        stage('cleaning image'){
            steps{
                script {
                     
                        sh " docker rmi nagui69/kaddem:abdelhak"
                    
                }
            }
        }
        stage('Get Previous Commit SHA') {
                when { branch 'Dev' }
                steps {
                    script {
                        previousCommitSHA = sh(script: 'git log -n 1 HEAD^ --format=%H', returnStdout: true).trim()
                        previousCommitShort = previousCommitSHA.take(8)
                        new_commitSHA         = "${env.GIT_COMMIT}"
                        new_commitShort       = new_commitSHA.take(8) 
                        echo "Previous Commit SHA: ${previousCommitShort}"
                        echo "New Commit SHA: ${new_commitShort}"
                    }
                }
            }
        stage('Apply Kubernetes files') {
            steps {
             withKubeConfig([credentialsId: 'kube' ]) {
              sh 'sed -i "s/abdelhak/dev${new_commitShort}/g" deploy.yaml'
              sh 'kubectl apply -f deploy.yaml'
            }
          }
        }
    }
}
