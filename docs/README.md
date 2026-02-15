# Project Documentation

## Overview
This folder contains high-level documentation and evidence for the end‑to‑end DevOps project: architecture description, runbooks, and screenshots/logs of successful runs. [web:37][web:40]

## Files
- `architecture.md` – describes the overall architecture:
  - Terraform builder EC2 instance
  - Flask application
  - Docker / Docker Hub
  - Jenkins (and Azure DevOps if used)
  - Kubernetes / Helm deployment
- `screenshots/` – screenshots of:
  - Successful `terraform apply` for the builder instance
  - SSH connection to the instance
  - Docker and Docker Compose versions on the instance
  - Jenkins pipeline run (all stages green)
  - Application running in the browser (listing EC2 instances, VPCs, etc.)
- Any additional markdown files explaining specific parts (for example `troubleshooting.md`, `design-decisions.md`).

## How to read this documentation
1. Start from `architecture.md` to understand the overall flow. [web:37]  
2. Use the screenshots to verify that each step (Terraform, Docker, CI/CD, deployment) has been executed successfully.  
3. Refer to troubleshooting notes if you try to reproduce the setup and face errors.

## Known gaps / partial implementations
If some parts of the exam are mocked or only partially implemented, they are listed here with an explanation of:
- What is missing
- Why it was not completed
- How it could be implemented in a next step
