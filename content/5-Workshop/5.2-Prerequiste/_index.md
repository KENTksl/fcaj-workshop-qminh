---
title : "Prerequisites"
date : 2024-01-01 
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---

## Objectives

Before starting the workshop, you need to fully prepare accounts, permissions, source code, and configuration files so that subsequent deployment steps can be executed consistently.

## Required components

- **AWS account** with permissions to create VPC, ECS, ECR, RDS, S3, CloudWatch, SNS, IAM, API Gateway, and related services.
- **GitHub repository** containing frontend source code, backend, Dockerfile, and configuration files such as `.github/workflows/*.yml`, `taskdef.json`, or environment files for deployment.
- **Application ready for packaging** to be built into Docker images.
- **Deployment parameters** such as region, naming convention, frontend/backend ports, environment variables, database information.
- **Architecture diagram** or design documentation for reference throughout the deployment process.

## IAM permissions suggestions

You can prepare the following permission groups or IAM roles:

- Role for GitHub Actions or process to push images to ECR.
- Role for GitHub Actions to build Docker images, log in to ECR, and update ECS service/task definition.
- Execution role and task role for ECS services.
- Permissions for CloudWatch, SNS, RDS monitoring and backup if you use those components.

## Inputs to prepare in advance

- Frontend and backend source code.
- Resource names according to unified convention, e.g., `globalmart-<service>-<env>`.
- Workflow files for GitHub Actions and ECS configuration.
- List of secrets or environment variables to include in runtime.

## Expected results

After this step, you are ready in terms of accounts, permissions, and deployment inputs to start building the infrastructure.

## Suggested images to add
- Hình repository GitHub của dự án.
![Overview](/images/5-Workshop/5.2-Prerequisite/repo-github.jpg)

- Hình các IAM user cho 5 thành viên của nhóm.
![Overview](/images/5-Workshop/5.2-Prerequisite/iam-users.jpg)
