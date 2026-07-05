---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Workshop to deploy GlobalMart infrastructure on AWS

#### Overview

This section serves as a deployment framework guide for the **GlobalMart** project based on the architecture diagram and content in the `Proposal` section. The workshop focuses on building DevOps/Platform Engineering infrastructure in a production-ready manner, including CI/CD pipelines, Multi-AZ networking layer, ECS runtime, secure data layer, centralized monitoring, and backup-alert mechanisms.

In each section, I've created a content template based on your project so you can add images, actual parameters, and deployment results later.

#### Workshop scope

- Prepare source code, IAM roles, ECR, and artifact bucket.
- Set up GitHub Actions, ECR, and ECS update flow to automate CI/CD.
- Build Multi-AZ VPC, NAT Gateway, route tables, and security groups.
- Deploy ECS Fargate for frontend/backend, Public ALB, Internal ALB, API Gateway, and VPC Link.
- Configure RDS MySQL Multi-AZ, Secrets Manager, and RDS Proxy.
- Set up CloudWatch Logs, metrics, alarms, SNS, and backups.
- Create testing framework, security review, and resource cleanup.

#### Content

1. [Workshop overview](5.1-Workshop-overview/)
2. [Set up AWS services](5.2-Prerequiste/)
3. [Set up source code, IAM OIDC, and GitHub Actions CI/CD](5.3-Build-Deploy/)
4. [Security, optimization, and extension checks](5.4-Policy/)
5. [Resource cleanup](5.5-Cleanup/)
6. [Actual results](5.6-Results/)
