## GitOps-Based CI/CD Pipeline with Cloud Build and Google Kubernetes Engine (GKE)

This repository contains the setup and code for implementing a **GitOps-based Continuous Delivery pipeline** with **Cloud Build** and **Google Kubernetes Engine (GKE)**. GitOps is a modern, reliable approach to Continuous Delivery that uses Git as the single source of truth for deployments, providing traceability, automation, and simplicity in the deployment lifecycle.

## Overview

In this configuration, I’ve designed a CI/CD pipeline that automatically builds container images from committed code, stores them in **Google Artifact Registry**, and updates Kubernetes deployment manifests stored in a Git repository. Once updated, the pipeline triggers an automatic deployment to **Google Kubernetes Engine (GKE)**.

The system uses two Git repositories:
1. **_app_ repository** – Stores the application code.
2. **_env_ repository** – Stores Kubernetes deployment manifests.

### How It Works

1. **Code Change**: A push to the _app_ repository triggers a Cloud Build pipeline. It builds a new container image, runs tests, and stores the image in Artifact Registry.
   
2. **Update Deployment Manifests**: After building the container image, Cloud Build updates the Kubernetes deployment manifests stored in the _env_ repository to reference the latest image version. The updated manifests are committed to the **candidate** branch of the _env_ repository.

3. **Deployment to Kubernetes**: Cloud Build triggers a deployment to **GKE** as part of the pipeline process. Once the deployment is successful, the updated manifests are moved to the **_production_ branch** of the _env_ repository, confirming the success of the deployment.

### Key Benefits of This GitOps Approach

- **Candidate Branch**: Provides a record of all deployments, including unsuccessful ones, for auditing and troubleshooting purposes.
- **Production Branch**: Reflects only successful deployments, serving as the definitive source of truth for production-ready configurations.
- **Rollback Capability**: If needed, any deployment can be rolled back by triggering a corresponding Cloud Build job, automatically updating the _production_ branch with the correct configuration.

### Monitoring and Rollback with Cloud Build

- **Cloud Build Visibility**: All builds are visible within **Cloud Build**, where I can track their status, whether they succeed or fail.
- **Rollback Process**: Should a deployment encounter issues, I can quickly roll back by triggering the previous successful deployment from Cloud Build, ensuring that the environment is always in a known, reliable state.

## Conclusion

This setup has streamlined the entire deployment pipeline, automating the process from code commit to deployment. By version-controlling every step in Git, I can easily manage deployments, track changes, and ensure that the application runs in a stable and predictable manner in GKE.

## References

- [Google Kubernetes Engine Pipeline using Cloud Build](https://www.cloudskillsboost.google/focuses/52829?parent=catalog)
