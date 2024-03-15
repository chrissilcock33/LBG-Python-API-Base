pipeline {
    agent any
    environment {
        GCR_CREDENTIALS_ID = 'jenkins-gcr'
        IMAGE_NAME = 'chris-s-lbg'
        GCR_URL = 'eu.gcr.io/lbg-mea-17'
        PROJECT_ID = 'lbg-mea-17'
        CLUSTER_NAME = 'chris-s-week3-project-cluster'
        LOCATION = 'europe-west2-c'
        CREDENTIALS_ID = 'jenkins-k8s'
    }
    stages {

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
        stage('Deploy to GKE') {

            steps {

                script {

                    // Deploy to GKE using Jenkins Kubernetes Engine Plugin
                    step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'kubernetes/deployment.yaml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
                }
            }
        }
    }
}
