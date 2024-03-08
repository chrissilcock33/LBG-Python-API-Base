pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh '''
                  sh "pwd"
                  sh "ls -a"
                  sh "sh setup.sh"
                '''
           }
        }
        stage('Deploy') {
            steps {
                sh '''
                    docker ps -a
                '''
            }
        }
    }
}
