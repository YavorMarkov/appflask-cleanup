pipeline {
    agent any

    environment {
        // Define your environment variables here
        PATH = "/opt/google-cloud-sdk/bin:${env.PATH}"
        BASE_IMAGE_NAME = 'appflask'
        GOOGLE_REGION = 'europe-west3'
        GOOGLE_PROJECT_ID = 'testjenkinsdeployappflask'
        GOOGLE_CLOUD_SERVICE_ACCOUNT_KEY = 'google-cloud-service-account-key'
        SERVICE_NAME = "${BASE_IMAGE_NAME}-service" // The name of the Google Cloud Run service to delete
    }

    stages {
        stage('Setup Google Cloud Environment') {
            steps {
                script {
                    // Authenticate with Google Cloud using the service account key
                    withCredentials([file(credentialsId: env.GOOGLE_CLOUD_SERVICE_ACCOUNT_KEY, variable: 'GOOGLE_APPLICATION_CREDENTIALS')]) {
                        sh '/opt/google-cloud-sdk/bin/gcloud auth activate-service-account --key-file="$GOOGLE_APPLICATION_CREDENTIALS"'
                    }
                    // Set the Google Cloud project
                    sh '/opt/google-cloud-sdk/bin/gcloud config set project ${GOOGLE_PROJECT_ID}'
                }
            }
        }

        stage('Delete Google Cloud Run Service') {
            steps {
                script {
                    // Command to delete the specified Google Cloud Run service
                    sh "/opt/google-cloud-sdk/bin/gcloud run services delete ${SERVICE_NAME} --platform=managed --region=${GOOGLE_REGION} --quiet"
                }
            }
        }
    }

    post {
        always {
            echo 'Cleanup process completed.'
        }
    }
}

