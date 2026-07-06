---
title: "Proposal"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# GlobalMart Cloud Computing Infrastructure
## Modern Production-Ready DevOps/Platform Engineering Infrastructure Solution: CI/CD Automation via GitHub Actions, Multi-Segment Isolated Network & Multi-Layer CloudWatch Monitoring

### 1. Project Summary
GlobalMart is a project entirely focused on researching, designing, and deploying a modern production-standard cloud infrastructure on the AWS platform. The project focuses on optimizing software operations lifecycle (DevOps) by eliminating traditional cumbersome AWS pipeline management services, replacing them with **GitHub Actions** combined with **IAM OIDC Role** secure authentication. The system applies a strict network segmentation model (VPC Public/Private Subnets), centralized API routing via **Amazon API Gateway / VPC Link**, configures advanced system logging (Container Logs), and sets up automatic secure backup scenarios to S3.

### 2. Problem Statement
### What's the Problem?
In real software projects, setting up loose network infrastructure, exposing databases to the internet, or deploying code manually is always the leading cause of data leaks and system downtime risks. Storing AWS Access Key/Secret Key directly on external CI/CD platforms carries the risk of exposing privileged accounts. Additionally, the lack of a centralized monitoring system paralyzes the engineering team's incident response capability when application containers encounter silent errors or crash suddenly.

### The Solution
The GlobalMart project sets up a standardized VPC network infrastructure consisting of Public Subnet A (hosting Internet ALB, NAT Gateway) and a system of Private Subnets to completely isolate the application execution environment (ECS Fargate) and data storage (RDS). The continuous integration and deployment (CI/CD) pipeline is built completely automatically via GitHub Actions, applying the **IAM OIDC Identity Provider** mechanism to grant secure permissions without using hardcoded keys. All incoming traffic to the system is centrally controlled via **API Gateway**, passing through the internal network thanks to **VPC Link / ALB Internal**. The system is monitored in multiple layers via a centralized CloudWatch Dashboard combined with automatic alerts through Amazon SNS sent directly to administrators' Email/SMS.

### Benefits and Return on Investment
The project brings significant practical value by streamlining the CI/CD engine, reducing operational costs (no need to pay for AWS CodeBuild/CodePipeline). Applying the Scan on push mechanism at ECR helps control malicious container code early. The system reaches full automation state: Engineers only need to perform `git push` to GitHub, and the system will automatically build images, update the new Task Definition Revision, and trigger **ECS Rolling Update** to update the website without service interruption (Zero-downtime).

### 3. Solution Architecture
The project architecture is set up synchronously on the AWS Fargate platform (Serverless Container) and a multi-layer security network model via Security Groups matrix. The infrastructure diagram is detailed below:

![GlobalMart Deployment Architecture](/images/2-Proposal/globalmart.png)

### AWS Services Used
- **AWS VPC & IAM OIDC**: Set up core network including Public Subnet (Internet routing) and isolated Private Subnets, combined with configuring Identity Provider to allow GitHub Actions to establish a secure trusted connection.
- **GitHub Actions (Build & Update Service)**: Acts as the core CI/CD tool, completely replacing AWS CodePipeline, CodeBuild, and CodeDeploy to automate the entire workflow.
- **Amazon ECR & S3 Artifact Bucket**: Docker Image repository integrated with security scanning (Scan on push) and storing system build state.
- **Amazon ECS Cluster & AWS Fargate**: Serverless container orchestrator managing two independent services `globalmart-frontend-task` and `globalmart-backend-task`.
- **AWS Application Load Balancer (ALB)**: System of 2 independent ALBs (ALB-Internet-Facing receiving public traffic to Frontend; ALB Internal receiving internal traffic routing to Backend).
- **Amazon API Gateway & VPC Link**: Centralized API control gateway, combined with VPC Link to securely forward traffic from Public environment to Internal Load Balancer located in Private zone.
- **Amazon RDS Single AZ**: MySQL/PostgreSQL database system completely hidden in Private Subnet B, absolutely blocking all direct access attempts from outside the Internet.
- **Amazon CloudWatch & Amazon SNS**: Pair collecting centralized multi-layer logs/metrics (ECS, ALB, RDS) and delivering automatic alerts via Email/SMS when the system is overloaded.

### Component Design
- **Multi-Layer Network & Security Infrastructure**: The system includes 3 separate Security Groups to isolate data flow. Frontend and Backend applications are placed in Private Subnet A network zone to hide origin IP, Database is located separately in Private Subnet B.
- **CI/CD Automation Pipeline via GitHub Actions**: Configure workflow `.yml` file handling sequentially: Checkout code -> Assume AWS Credentials via OIDC Role -> Build and tag Docker Image with Git Commit SHA -> Push to ECR -> Automatically generate new Task Definition JSON file -> Trigger `update-service` command on ECS.
- **Data Connection & Operations**: User calls API via API Gateway -> Passes through VPC Link -> To ALB Internal -> To ECS Backend -> Directly queries Single AZ RDS Database via secure internal network connection.
- **Centralized Monitoring & Logging**: CloudWatch Logs collects all command-line log data (Console logs) from Frontend/Backend containers, routing traces at ALB, and RDS Performance state to display visually on a centralized CloudWatch Dashboard.

### 4. Technical Implementation
**Implementation Phases**
The project execution process is planned in detail over a 1-month period, entirely focusing on configuration and system infrastructure integration steps:
- **Week 1 - Core Network & IAM OIDC Role Setup**: Initialize VPC core network including Public Subnet system (hosting ALB Internet-Facing, NAT Gateway) and isolated Private Subnets. Configure Identity Provider (IdP) on IAM attached to GitHub's OIDC Provider, create IAM Role allowing GitHub Actions account to interact with ECR and ECS without hardcoded Access Key.
- **Week 2 - Containerization & GitHub Actions Workflow Build**: Initialize Frontend and Backend ECR repositories with scan on push enabled. Write workflow configuration file `.github/workflows/deploy.yml` on GitHub to automate the workflow: use docker build command, tag image with `${{ github.sha }}` identifier, and perform push to Amazon ECR.
- **Week 3 - Initialize Container Runtime, API Gateway & Routing**: Initialize ECS Fargate Cluster along with detailed Task Definitions system. Set up ALB Internet-Facing (pointing to Frontend Target Group) and ALB Internal (pointing to Backend Target Group). Configure API Gateway combined with VPC Link directly connecting to ALB Internal to open a secure API path for the application.
- **Week 4 - Data Layer, Monitoring System & SNS Alerts**: Initialize Single AZ RDS configuration located in Private Subnet B, pass secure connection environment variables from Backend. Configure CloudWatch Logs logging system, centralized Dashboard visually displaying Container CPU/Memory Utilization metrics and RDS performance. Set up 6 CloudWatch Alarms sending automatic alerts via Amazon SNS to phone/email when system overload incidents occur.

**Technical Requirements**
- **GitHub Actions Automation Skills**: Understand how to write YAML workflows, manage secure GitHub Secrets, declare environment variables, and use AWS CLI combined with Python to cook Task Definition (`register-task-definition`).
- **Advanced Networking & Routing Thinking**: Understand API Gateway routing mechanism, VPC Link, how to configure cross-Security Group permissions (allowing ALB to access Container, allowing Backend to connect to correct Database RDS Port).

### 5. Timeline & Milestones
**Project Timeline**
The project execution roadmap is broken down and closely follows real infrastructure deployment progress within 1 month:
- Milestone 1 (End of Week 1): Complete setting up the entire secure VPC network core, smoothly configure IAM OIDC Role secure connection between GitHub and AWS.
- Milestone 2 (End of Week 2): Complete GitHub Actions workflow configuration, successfully package and push Frontend/Backend Docker Image versions to Amazon ECR.
- Milestone 3 (End of Week 3): Successfully deploy ECS Fargate Cluster, finish configuring API Gateway system, VPC Link, and multi-layer ALB load balancers, ensuring smooth web routing.
- Milestone 4 (End of Week 4): Bring Single AZ RDS data layer into operation, activate CloudWatch Dashboard multi-layer monitoring system and automatic alerting system via SNS. Successfully execute scenario checking actual email alert activation and project acceptance.

### 6. Budget Estimation
### Infrastructure Costs
- AWS Services:
    - Amazon ECS Fargate (Frontend): $4.25/month (1 task, 0.25 vCPU, 512 MB, 24/7).
    - Amazon ECS Fargate (Backend): $10.36/month (1 task, 0.5 vCPU, 1 GB, 24/7).
    - Public IPv4 (ALB + NAT): $7.30/month (2 public IPs × $0.005/hour).
    - NAT Gateway: $43.37/month (730 hours, ~5 GB data processed).
    - ALB Internet-facing: $24.24/month (730 hours, ~1 LCU/hour).
    - ALB Internal: $19.40/month (730 hours, minimal LCU).
    - Amazon API Gateway (HTTP API): $1.00/month (~1 million requests/month).
    - Data Transfer Out: $0.45/month (~5 GB outbound to Internet).
    - Amazon RDS Single AZ (db.t3.micro): $17.42/month (24/7 Dev/Test environment).
    - RDS Storage: $2.76/month (20 GB gp2).
    - Amazon ECR (Frontend + Backend): $0.58/month (4 GB storage, ~100 pulls).
    - Backup Bucket (S3): $0.58/month (20 GB snapshot/export storage).
    - CloudWatch Logs: $3.95/month (5 GB ingestion, container + ALB logs).
    - CloudWatch Metrics & Alarms: $4.95/month (15 metrics, 6 alarms).
    - CloudWatch Dashboard: $3.00/month (1 dashboard, 5 main widgets).
    - Amazon SNS: $0.00/month (under 1,000 email notifications, in AWS Free Tier).
    - GitHub Actions Runner: $0.00/month (Using GitHub's 2,000 free minutes/month for private repository).

Total: **$187.01 / month**

### 7. Risk Assessment
#### Risk Matrix
- Character escape errors or incorrect JSON structure when automatically creating Task Definition via CLI: Medium impact, medium probability.
- Data leak risk due to accidentally opening Database port to Internet: High impact, low probability.
- Deployment flow stuck or hanging due to Target Group Health Check misconfigured success codes: Medium impact, medium probability.

#### Mitigation Strategies
- Task Definition creation errors: Thoroughly handled by exporting JSON configuration string from Python script to temporary `*.json` file before feeding into AWS CLI `--cli-input-json` command in workflow.
- Database security: RDS is placed completely in Private Subnet B without internet gateway, while RDS Security Group only opens a single port allowing internal IP from ECS Backend Task Security Group.
- Health Check stuck errors: Configure success codes to relax to `200-499` in Target Group (`tg-backend` and `tg-frontend`) to accept any initial startup responses from Spring Boot/Node.js.

#### Contingency Plans
- Trigger manual RDS Snapshot schedule before each major code push, ensuring data rollback capability to the nearest stable state within 5 minutes if new code breaks DB structure.
- Set up automatic backup export scenario (Backup Export) to an independent S3 Backup Bucket with lifecycle storage feature enabled to optimize long-term storage costs.

### 8. Expected Outcomes
#### Technical Improvements:
Successfully build an automated, streamlined, and modern security-standard DevOps infrastructure: master IAM OIDC Role authentication flow without keys, securely control traffic via API Gateway/ALB, and successfully set up CloudWatch Dashboard monitoring system combined with proactive Email alerts when container overload incidents occur.
#### Long-term Value
Bring a standardized architecture sample document (Blueprint) about building CI/CD pipeline directly integrating GitHub Actions and AWS ECS Fargate; create a solid, easy-to-maintain, cost-optimized infrastructure foundation for enterprises to be ready to deploy full-stack Microservices applications later.
