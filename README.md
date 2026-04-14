[![Open in Codespaces](https://classroom.github.com/assets/launch-codespace-2972f46106e565e64193e422d61a12cf1da4916b45550586e14ef0a7c637dd04.svg)](https://classroom.github.com/open-in-codespaces?assignment_repo_id=23254489)

# CSSE6400 Week 5 Practical

This repository deploys TaskOverflow to AWS using Terraform.

The practical now includes:

- An AWS RDS PostgreSQL database
- An AWS ECS Fargate service
- An AWS ECR repository for the application image

## Project Files

- `main.tf`: AWS infrastructure for RDS, ECS, security groups, IAM/VPC data sources, and ECR
- `image.tf`: Docker build and push configuration for ECR
- `Dockerfile`: production image used by ECS

## Prerequisites

- Terraform installed
- Docker Desktop running
- AWS Learner Lab started
- AWS CLI credentials copied into a local `credentials` file in the repository root

The `credentials` file is ignored by git and must not be committed.

## AWS Provider Setup

Terraform uses the AWS provider in `us-east-1` and reads credentials from:

```text
./credentials
```

## How To Deploy

1. Start the AWS Learner Lab.
2. Copy the AWS CLI credentials block into `credentials`.
3. Initialise Terraform:

```bash
terraform init -upgrade
```

4. Review the deployment plan:

```bash
terraform plan
```

5. Apply the infrastructure:

```bash
terraform apply -auto-approve
```

This will:

- create the PostgreSQL RDS instance
- create the ECR repository
- build and push the Docker image to ECR
- create the ECS cluster, task definition, and service

## Important Note About Image Architecture

The development machine used for this submission is Apple Silicon (`arm64`), while ECS Fargate in this deployment runs `x86_64`.

To make the container run correctly on ECS, the Docker image is built for:

```text
linux/amd64
```

This is configured in `image.tf`.

## Validation Performed

The following checks were completed:

- `terraform init`
- `terraform init -upgrade`
- `terraform plan`
- `terraform apply -auto-approve`
- ECS service verification through AWS CLI
- HTTP verification against the deployed application

## Deployment Result

The deployed TaskOverflow application was verified to return `HTTP 200 OK` from ECS after switching to the ECR-hosted image.

Example verified endpoint during testing:

```text
http://44.222.98.151:6400
```

Note that the public IP may change when ECS replaces tasks.

## Cleanup

To destroy the deployed infrastructure:

```bash
terraform destroy -auto-approve
```
