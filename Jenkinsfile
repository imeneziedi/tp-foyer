pipeline {
    agent any
    stages {
        stage('maven clean ') {
            steps {

                sh '''
                    mvn clean
                    mvn compile 
                    '''
            }
        }

    }
}
