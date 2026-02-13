# Terraform Infrastructure

## What is implemented

The actual Terraform configuration is located in the folder `../builder-EllaHalevy/` and includes:

- `main.tf` – EC2 instance, security group, Docker and Docker Compose installation.
- `provider.tf` – AWS provider configuration.
- `var.tf` – Input variables.
- `outputs.tf` – Outputs for instance ID and public key name.

This configuration provisions an AWS EC2 instance prepared to run the Flask application using Docker/Docker Compose.

## How to run

From the folder `builder-EllaHalevy`:

```bash
terraform init
terraform plan
terraform apply
