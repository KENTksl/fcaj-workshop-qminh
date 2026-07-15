---
title : "Workshop overview"
date : 2026-06-30
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

## Workshop objectives

This workshop guides the infrastructure deployment for the **GlobalMart** project according to the direction presented in the `Proposal` section: automating the CI/CD pipeline via **GitHub Actions** with secure **IAM OIDC** authentication, running frontend and backend applications on **Amazon ECS Fargate**, organizing isolated VPC networking with public/private subnets, routing centralized API traffic through **Amazon API Gateway** and **VPC Link**, setting up automated email notifications upon pipeline completion, and implementing centralized observability with **CloudWatch Dashboards & Alarms**.

## Overall architecture

The system can be divided into 5 main component groups:

- **Source and CI/CD**: Developers push code to GitHub. GitHub Actions automatically triggers workflows, authenticates securely via IAM OIDC Roles, builds Docker images, pushes them to Amazon ECR, and orchestrates ECS Rolling Updates while sending real-time email status notifications.
- **Application runtime**: Frontend and Backend tasks run as serverless containers on Amazon ECS Fargate inside an isolated Private Subnet, managed independently for high security and efficiency.
- **Networking and access**: Public ALB directly serves internet requests to the Frontend container. All API traffic from the public domain is controlled by Amazon API Gateway and securely forwarded via VPC Link to the Backend container through an Internal ALB.
- **Data layer**: The Backend task connects securely via internal networking to an Amazon RDS Single AZ database tucked deep inside Private Subnet B, blocking all direct access attempts from the public Internet.
- **Monitoring and backup**: CloudWatch dynamically aggregates container logs, routing metrics, and database performance data into a centralized dashboard, working alongside Amazon SNS to dispatch critical email/SMS alerts during system overloads.

## Deployment scope

In this workshop, you will deploy according to the following step groups:

1. Prepare the GitHub repository, configure AWS IAM OIDC Identity Providers, create execution roles, and set up Amazon ECR repositories.
2. Build and implement the GitHub Actions deployment workflow (`.yml`) to handle automated building, tagging, and pushing of container images.
3. Establish the core network architecture including the VPC, Public and Private Subnets, Route Tables, and a NAT Gateway.
4. Set up the network access matrix by configuring 3 isolated Security Groups to manage strict inbound traffic between layers.
5. Provision the ECS Fargate Cluster, Task Definitions, and ECS Services, integrated with an Internet-facing ALB, an Internal ALB, an API Gateway, and a VPC Link.
6. Launch the Amazon RDS Single AZ instance, map the internal application connectivity, and structure automatic backup routines.
7. Build the centralized CloudWatch Log Groups, custom Dashboards, active metric Alarms, and SNS email subscriptions for complete observability.

## Expected results

After completing the workshop, you will have:

- A deployment documentation framework for the entire project infrastructure.
- Clear configuration sequence between components in the architecture.
- Placeholder sections to add actual images from your deployment process.

## Sơ đồ kiến trúc tổng thể mới của dự án

![Overview](/images/5-Workshop/5.1-Workshop-overview/globalmart.png)