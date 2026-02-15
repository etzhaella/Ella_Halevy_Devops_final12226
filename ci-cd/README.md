# CI/CD Pipelines

## Overview
This folder contains the CI/CD configuration for the project.  
The main pipeline is implemented in Jenkins and can optionally be mirrored in Azure DevOps.

## Jenkins pipeline (Jenkinsfile)
The `Jenkinsfile` defines a declarative pipeline with the following stages:
- Checkout source code from GitHub
- Linting and basic tests (Python / YAML linters, etc.)
- Security scan (for example: `bandit` / `trivy` on the Docker image) in parallel with linting
- Build Docker image for the Flask application
- Push the image to Docker Hub
- (Optional) Deploy stage – uses Helm or Docker Compose to deploy the new image

### How to run the Jenkins pipeline
1. Create a new **Multibranch Pipeline** or **Pipeline** job in Jenkins.  
2. Point it to this GitHub repository and to the branch you want to build.  
3. Make sure the required credentials (GitHub, Docker Hub, AWS/Kubernetes if needed) are configured in Jenkins and referenced in the `Jenkinsfile`.  
4. Trigger a build (manually or on push).  
5. After a successful run:
   - A new Docker image should be available in Docker Hub.
   - If the deploy stage is enabled, the application will be updated in the target environment.



## Notes / Limitations
- Direct pushes to `main` are blocked; pipelines should usually run on feature branches and `dev`, then on `main` after merge.  
- If any stage is mocked or not implemented yet (for example, deploy), it is clearly commented inside the `Jenkinsfile` and explained here.
