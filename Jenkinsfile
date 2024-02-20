pipeline {
    agent any

    environment {
        // Define your environment variables here
        PATH = "/opt/google-cloud-sdk/bin:${env.PATH}"
        BASE_IMAGE_NAME = 'appflask'
        GOOGLE_REGION = 'europe-west3'
        GOOGLE_PROJECT_ID = 'testjenkinsdeployappflask'
        GOOGLE_CLOUD_SERVICE_ACCOUNT_KEY = 'google-cloud-service-account-key'
        // Define the service name to be deleted
        SERVICE_NAME = "${BASE_IMAGE_NAME}-service"
    }

    stages {
        stage('Authenticate with Google Cloud') {
            steps {
                script {
                    // Securely handling Google Cloud credentials
                    withCredentials([file(credentialsId: env.GOOGLE_CLOUD_SERVICE_ACCOUNT_KEY, variable: 'GOOGLE_APPLICATION_CREDENTIALS')]) {
                        // Authenticate with Google Cloud using the service account key file
                        sh '/opt/google-cloud-sdk/bin/gcloud auth activate-service-account --key-file="$GOOGLE_APPLICATION_CREDENTIALS"'
                    }
                }
            }
        }

        stage('Delete Google Cloud Run Service') {
            steps {
                script {
                    // Command to delete the Cloud Run service
                    sh "/opt/google-cloud-sdk/bin/gcloud run services delete ${SERVICE_NAME} --platform=managed --region=${GOOGLE_REGION} --project=${GOOGLE_PROJECT_ID} --quiet"
                }
            }
        }

        stage('Cleanup Docker Images') {
            steps {
                script {
                    // Optionally, add commands here to delete Docker images from Google Container Registry or Google Artifact Registry if needed.
                    // Example for deleting images from Google Container Registry:
                    // sh "gcloud container images delete gcr.io/${GOOGLE_PROJECT_ID}/${BASE_IMAGE_NAME} --quiet"
                }
            }
        }
    }

    post {
        always {
            // Add any cleanup actions that should always run here
            // For example, logging out of Google Cloud if necessary
        }
    }
}

