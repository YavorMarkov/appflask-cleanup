# Google Cloud Run Service Cleanup Pipeline

This README outlines the Jenkins pipeline designed for the specific purpose of cleaning up a Google Cloud Run service. This pipeline automates the deletion process, ensuring that specific resources within a Google Cloud Project are removed without impacting the project itself or other associated resources.

## Overview

The pipeline is configured to authenticate with Google Cloud using a service account, set the project context, and then execute the deletion of a specified Google Cloud Run service. It is designed for scenarios where you need to clean up or remove services as part of CI/CD workflows, post-testing cleanups, or environment resets.

## Environment Configuration

The pipeline utilizes several environment variables for its operation:

- `PATH`: Enhanced to include the path to the Google Cloud SDK, ensuring commands are executable.
- `BASE_IMAGE_NAME`: Specifies the base name of the Docker image associated with the Google Cloud Run service, aiding in naming conventions.
- `GOOGLE_REGION`: The Google Cloud region where the service is deployed.
- `GOOGLE_PROJECT_ID`: The ID of the Google Cloud Project within which the service exists.
- `GOOGLE_CLOUD_SERVICE_ACCOUNT_KEY`: The Jenkins credential ID for the Google Cloud service account key, used for authentication.
- `SERVICE_NAME`: The name of the Google Cloud Run service to be deleted, typically derived from the `BASE_IMAGE_NAME`.

## Pipeline Stages

### Setup Google Cloud Environment

- **Purpose**: Prepares the Google Cloud environment for operational commands.
- **Actions**: Authenticates with Google Cloud using the specified service account and sets the project context to the provided `GOOGLE_PROJECT_ID`.

### Delete Google Cloud Run Service

- **Purpose**: Deletes the specified Google Cloud Run service.
- **Actions**: Executes the `gcloud run services delete` command with the `SERVICE_NAME`, ensuring deletion within the specified `GOOGLE_REGION`. The `--quiet` flag is used to bypass any interactive prompts, facilitating automated execution.

## Post-execution Actions

- **Always**: Outputs a completion message indicating the cleanup process is finished.

## Considerations

- **Permissions**: Ensure the service account has adequate permissions to delete services in Google Cloud Run.
- **Service Dependencies**: Review any dependencies or related services that might be affected by the deletion of the target service.
- **Irreversible Actions**: Deletion of a service cannot be undone. Proceed with caution, especially in production environments.

## Usage

To use this pipeline, ensure your Jenkins environment is configured with access to the Google Cloud SDK and that the necessary credentials are available as Jenkins secrets. Update the environment variables within the Jenkinsfile to match your specific Google Cloud project and service details.

This pipeline is designed for flexibility and can be adapted to include additional cleanup tasks or to handle other resources within Google Cloud as needed.


🔍 Actively seeking job opportunities to leverage my DevOps skills. Eager to contribute to a dynamic team 🔍