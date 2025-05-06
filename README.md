# GitOps-style Continuous Delivery with Cloud Build

This repository contains the setup and code I used for implementing a **GitOps-style Continuous Delivery pipeline** with **Cloud Build** and **Google Kubernetes Engine (GKE)**. GitOps is a powerful and popular approach to Continuous Delivery that uses Git as a source of truth for deployments, making the process more reliable and auditable.

## Overview

In this setup, I created a CI/CD pipeline that automatically builds container images from committed code, stores those images in **Google Artifact Registry**, and updates the Kubernetes deployment manifests stored in a Git repository. Once updated, the pipeline triggers a deployment to **Google Kubernetes Engine (GKE)**.

This setup uses two Git repositories:
1. **_app_ repository** – Stores the application code.
2. **_env_ repository** – Stores Kubernetes deployment manifests.

### How it Works

1. **Code Change**: When I push a change to the _app_ repository, a Cloud Build pipeline is triggered. It runs tests, builds a container image, and stores it in Artifact Registry.
   
2. **Update Deployment Manifests**: After pushing the new image, Cloud Build updates the Kubernetes manifest in the _env_ repository to use the latest image version. The updated manifests are committed to the _candidate_ branch of the _env_ repository.

3. **Deployment to Kubernetes**: A deployment to **GKE** is triggered as part of the pipeline process. After the deployment succeeds, the updated manifests are copied to the **_production_ branch** of the _env_ repository, indicating that the deployment was successful.

### Benefits of This Approach

- **Candidate Branch**: Tracks all deployment attempts, including failed ones, providing a complete history of deployments.
- **Production Branch**: Reflects only the successful deployments, making it the source of truth for production-ready configurations.
- **Rollback**: Any deployment can be rolled back by re-executing the corresponding Cloud Build job, which will update the _production_ branch to reflect the rollback, ensuring the correct deployment history.

### Cloud Build & Deployment Visibility

- I have visibility into all deployments within **Cloud Build**, where I can track the status of each build, including whether it succeeded or failed.
- If needed, I can easily roll back to any previous deployment by triggering the respective build job in Cloud Build, ensuring that my environment is always in a known, reliable state.

## Conclusion

With this setup, I’ve streamlined my deployment process, automating the flow from code commit to deployment. Everything is version-controlled in Git, providing clear visibility and easy rollback in case of any issues.
