pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo "Building..."
                sh "sh setup.sh"
           }
        }
        stage('Deploy') {
            steps {
                sh "docker ps -a"
            }
        }
    }
}
