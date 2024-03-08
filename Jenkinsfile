pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "lbg"
        DOCKER_USER = "chrissilcock33"
        PORT = 5001
    }
    stages {
        stage('Cleanup') {
            steps {
                //echo "Building..."
                //sh "sh setup.sh"
                echo "Cleaning up previous build artifacts..."
                sleep 3
               
                sh "docker ps -a"
                sh "docker stop \$(docker ps -q) || sleep 1"
                sh "docker rm \$(docker ps -qa) || sleep 1"
                echo "Cleanup done."
           }
        }

        stage('Build and Push') {
            steps {
                echo "Building the Docker image..."
                sleep 3
                sh "docker build -t $DOCKER_USER/$DOCKER_IMAGE ."
                sh "docker push $DOCKER_USER/$DOCKER_IMAGE"
            }
        }
        stage('modify') {
            steps {
                echo "Modifying the application..."
                sleep 3
                sh "export PORT=$PORT"
                echo "Modifications done. Port is now set to $PORT"
        }
        }
        stage('Deploy') {
            steps {
                sh "docker ps -a"
                sh "docker run -d -p 80:$PORT $DOCKER_USER/$DOCKER_IMAGE"
            }
        }
    }
}
