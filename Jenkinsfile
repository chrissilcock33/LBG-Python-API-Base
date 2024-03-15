pipeline {
    agent any
    environment {
        //DOCKER_IMAGE = "lbg"
        //DOCKER_USER = "chrissilcock33"
        //PORT = 5001
        GCR_CREDENTIALS_ID = 'jenkins-gcr' // The ID you provided in Jenkins credentials
        //chris-s-storage-admin@lbg-mea-17.iam.gserviceaccount.com

        IMAGE_NAME = 'chris-s-lbg'
        GCR_URL = 'gcr.io/lbg-mea-17'
    }
    stages {
        // stage('Cleanup') {
        //     steps {
        //         //echo "Building..."
        //         //sh "sh setup.sh"
        //         echo "Cleaning up previous build artifacts..."
        //         sleep 3
               
        //         sh "docker ps -a"
        //         sh "docker stop \$(docker ps -q) || sleep 1"
        //         sh "docker rm \$(docker ps -qa) || sleep 1"
        //         echo "Cleanup done."
        //    }
        // }

        stage('Build and Push to GCP') {
            steps {
                echo "Auth to GCP"
                withCredentials([file(credentialsId: GCR_CREDENTIALS_ID, variable: 'GOOGLE_APPLICATION_CREDENTIALS')]) {
                    sh 'gcloud auth activate-service-account --key-file=$GOOGLE_APPLICATION_CREDENTIALS'
                }

                echo "Configure Docker to use gcloud as a credential helper"
                sh 'gcloud auth configure-docker --quiet'

                echo "Building the Docker image..."
                sh "docker build -t ${GCR_URL}/${IMAGE_NAME}:${BUILD_NUMBER} ."

                echo "Push the Docker image to GCR"
                sh "docker push ${GCR_URL}/${IMAGE_NAME}:${BUILD_NUMBER}"
            }
        }
        stage('modify') {
            steps {
                echo "Modifying the application..."
                sleep 3
                echo "Modifications done."
                echo "Doing a thing"
        }
        }
        stage('Deploy') {
            steps {
                // sh "docker ps -a"
                // sh "docker run -d -p 80:$PORT -e PORT=$PORT $DOCKER_USER/$DOCKER_IMAGE"
                // sh "docker ps -a"
                echo "done"
            }
        }
    }

