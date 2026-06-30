---
title: "Proposal"
date: 2026-07-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# GlobalMart Cloud Computing Infrastructure
## Production-Ready DevOps/Platform Engineering Infrastructure Setup Solution: CI/CD Automation, Multi-Layer Monitoring, and High Availability Assurance

### 1. Project Summary
GlobalMart is a project focused entirely on researching, designing, and deploying a production-standard cloud infrastructure on the AWS platform. The core goal of the project is not to develop application source code logic (software code), but to focus on automating the software operations lifecycle (DevOps). The project applies Multi-AZ infrastructure setup techniques, builds an automated 3-phase secure CI/CD pipeline, manages intelligent data connections via Proxy, configures advanced system logging (VPC Flow Logs, Container Logs) and simulates disaster recovery drills in under 60 seconds.

### 2. Problem Statement
### What’s the Problem?
In real software projects, the lack of a standardized network infrastructure, loose security layering, or resource allocation in a single availability zone (Single-AZ) is always the leading cause of system downtime risks. Manual container build and deploy processes are time-consuming, prone to source code leaks, and cannot control security vulnerabilities. Additionally, the lack of a centralized monitoring system that collects both network logs (VPC Flow Logs) and application logs paralyzes the engineering team's incident response capability when the system encounters errors or attacks.

### The Solution
The GlobalMart project focuses on setting up a VPC Multi-AZ network infrastructure (4 Subnets, 2 NAT Gateways) to completely isolate the execution environment and data storage. The permission process is tightly controlled from the beginning with a detailed IAM Roles system for each task (Pipeline, Build, ECS, Deploy, RDS Monitoring). The continuous integration and deployment (CI/CD) pipeline is built automatically via AWS CodePipeline combined with CodeBuild (Privileged mode enabled for Docker packaging) and CodeDeploy (using a secure Blue/Green strategy). All data state is protected by an RDS MySQL Multi-AZ cluster along with AWS RDS Proxy to optimize connection pooling. The system is monitored in multiple layers via a centralized CloudWatch Dashboard combined with automatic alerts through Amazon SNS.

### Benefits and Return on Investment
The project brings high practical value through optimizing operations processes and standardizing infrastructure security without depending on application source code. Automating security scanning when pushing images (Scan on push in ECR) helps early detect malicious code vulnerabilities. Applying the RDS Proxy connection mechanism protects the database from connection overload risks. Most importantly, failure drill scenarios help prove that the infrastructure can automatically recover and failover safely in a very short time (<60 seconds), minimizing system downtime on the Production environment.

### 3. Solution Architecture
The project architecture is set up synchronously across 2 Availability Zones, Availability Zone A and Availability Zone B, to meet High Availability criteria. Network layers, container orchestration, routing, and data storage are all clearly separated via a secure Security Groups permission matrix. The infrastructure diagram is detailed below:

![GlobalMart Deployment Architecture](/images/2-Proposal/GlobalMart.png)

### AWS Services Used
- **AWS VPC & IAM**: Set up core 4-Subnet network, 2 SPOF-preventing NAT Gateways, 2 separate Private Route Tables, and a detailed IAM Roles permission matrix.
- **AWS CodePipeline, CodeBuild & CodeDeploy**: Trio of services building an automated 3-phase pipeline (Source → Build → Deploy) and secure Blue/Green deployment.
- **Amazon ECR & S3 Artifact Bucket**: Docker Image repository integrated with security scanning (Scan on push) and build state storage with 30-day lifecycle policy and versioning enabled.
- **Amazon ECS Cluster & AWS Fargate**: Serverless container orchestration integrated with deep Container Insights monitoring.
- **AWS Application Load Balancer (ALB)**: System of 2 independent ALBs (Public ALB spanning 2 AZs with 2 Target Groups for Blue/Green; Internal ALB spanning 2 AZs for internal Frontend and Backend connection).
- **Amazon RDS for MySQL, Secrets Manager & RDS Proxy**: Multi-AZ database cluster securing information via Secrets Manager, mandatory indirect communication through RDS Proxy layer for transparent failover optimization.
- **Amazon CloudWatch & Amazon SNS**: Pair collecting centralized multi-layer logs/metrics and delivering automatic alerts via Email/SMS.

### Component Design
- **Network & Security Infrastructure**: Set up VPC network including 2 Public Subnets and 2 Private Subnets. Use 6 separate Security Groups to isolate each component (including sg-rds-proxy). Frontend application is placed in Private Subnet A (AZ-A) and Backend is located in Private Subnet B (AZ-B).
- **CI/CD Automation Pipeline**: Configure declaration files `buildspec.yml`, `appspec.yml`, `taskdef.json` directly in the GitHub repo for CodePipeline orchestration. The build process in CodeBuild runs in Privileged mode for packaging Frontend and Backend Docker Images.
- **Data Connection & Operations**: Initialize DB Subnet Group covering both AZs. Backend layer only connects securely to the database via RDS Proxy Endpoint, absolutely blocking all direct connection attempts to RDS MySQL.
- **Monitoring & Logging**: CloudWatch Logs collects all data from Containers, network flow data via VPC Flow Logs, and routing traces at ALB. Configure centralized Multi-AZ CloudWatch Dashboard displaying visual 9 metrics widgets.
- **Backup & Recovery Drills**: System automatically sets up RDS Automated Backup policy within 7 days and safely transfers to an independent S3 Backup Bucket.

### 4. Technical Implementation
**Implementation Phases**
The project execution process is planned in detail over a 1-month period, focusing entirely on configuration and system infrastructure integration steps:
- **Week 1 - Core Network & Security Layering**: Research requirements, set up detailed IAM Roles permission system. Initialize VPC Multi-AZ core network including 4 Subnets, configure 2 independent NAT Gateways to avoid Single Points of Failure (SPOF) and assign 2 separate Private Route Tables for each AZ. Build upfront security matrix with 6 Security Groups (including sg-rds-proxy).
- **Week 2 - Containerization & CI/CD Automation Preparation**: Initialize Frontend and Backend ECR repositories with scan on push enabled. Write pipeline configuration and definition files (`buildspec.yml`, `appspec.yml`, `taskdef.json`). Initialize S3 Artifact Bucket with versioning and 30-day lifecycle policy enabled. Configure CodeBuild Project with Privileged mode for Docker packaging.
- **Week 3 - Initialize Container Runtime & Traffic Routing**: Configure complete 3-phase CodePipeline. Initialize ECS Fargate Cluster integrated with Container Insights along with detailed Task Definitions. Set up Public ALB pair (attached with 2 Target Groups for Blue/Green) and Internal ALB running in parallel across 2 AZs. Deploy advanced CodeDeploy configuration.
- **Week 4 - Set Up HA Data Layer, Monitoring System & Disaster Drills**: Create DB Subnet Group, set up RDS MySQL Multi-AZ system (Primary AZ-A + Standby AZ-B) securing information via Secrets Manager. Integrate RDS Proxy configuration managing connection pooling. At the same time, configure CloudWatch Logs, centralized Dashboard (9 widgets) along with 8 automatic Alarms via SNS. Execute DR Drill scenario (RDS Failover under 60 seconds) and perform full system End-to-End Test before acceptance.

**Technical Requirements**
- **Infrastructure Configuration & IaC Skills**: Understand clearly how to network, write and optimize intermediate configuration files for container system (`taskdef.json`) and pipeline orchestration (`appspec.yml`).
- **Security & Monitoring Thinking**: Ability to separate privileges (Least Privilege), understand clearly how VPC Flow Logs work and set up logical filter matrix for alert conditions (CloudWatch Alarms) corresponding to cloud system disaster errors.

### 5. Timeline & Milestones
**Project Timeline**
The project execution roadmap is broken down and closely follows the real infrastructure deployment progress within 1 month:
- Milestone 1 (End of Week 1): Complete setting up the entire secure VPC Multi-AZ network core, Security Groups matrix to eliminate single points of failure at the edge network layer.
- Milestone 2 (End of Week 2): Complete configuration and successful integration of container image repositories (ECR), prepare all configuration definition files for automating the packaging flow.
- Milestone 3 (End of Week 3): Fully automated CodePipeline CI/CD flow goes into stable operation, successfully deploy ECS Fargate cluster and multi-region ALB load balancer pair.
- Milestone 4 (End of Week 4): Bring secure Multi-AZ data layer, RDS Proxy system and CloudWatch multi-layer monitoring into operation. Successfully execute practical disaster recovery scenarios (DR Drill) under 60 seconds and accept project.

### 6. Budget Estimation
### Infrastructure Costs
- AWS Services:
    - Amazon ECS Fargate (Frontend): $8.50/month (2 tasks, 0.25 vCPU, 512 MB, 24/7).
    - Amazon ECS Fargate (Backend): $20.73/month (2 tasks, 0.5 vCPU, 1 GB, 24/7).
    - Public IPv4 (ECS + ALB + NAT): $29.20/month (8 public IPs × $0.005/hour).
    - NAT Gateway (AZ-A): $43.37/month (730 hours, ~5 GB data processed).
    - NAT Gateway (AZ-B): $43.37/month (730 hours, ~5 GB data processed).
    - ALB Internet-facing: $24.24/month (730 hours, ~1 LCU/hour).
    - ALB Internal: $19.40/month (730 hours, minimal LCU).
    - Data Transfer Out: $0.45/month (~5 GB outbound to Internet).
    - Data Transfer Cross-AZ: $0.10/month (~10 GB Frontend → Backend in different AZ).
    - RDS MySQL Multi-AZ (db.t3.micro): $34.84/month (Primary + Standby, 24/7).
    - RDS Storage: $5.52/month (20 GB gp2 × 2 AZ).
    - RDS Proxy: $21.90/month (2 vCPU minimum, 730 hours).
    - Secrets Manager: $0.41/month (1 secret, API calls).
    - RDS Snapshot Export to S3: $0.24/month (~20 GB exported).
    - CodePipeline: $1.00/month (1 active pipeline).
    - CodeBuild: $1.25/month (~50 builds × 5 minutes, general1.small).
    - Amazon ECR (Frontend + Backend): $0.58/month (4 GB storage, ~100 pulls).
    - S3 Artifact Bucket: $0.04/month (1 GB, 1,000 requests).
    - S3 Backup Bucket: $0.58/month (20 GB, transition to Glacier after 30 days).
    - CloudWatch Logs: $3.95/month (5 GB ingestion, container + ALB + VPC Flow Logs).
    - CloudWatch Metrics & Alarms: $6.60/month (20 metrics, 8 alarms).
    - CloudWatch Dashboard: $3.00/month (1 dashboard, 9 widgets).
    - AWS Backup: $1.95/month (daily + weekly backup plan, ~30 GB vault).
    - Amazon SNS: $0.00/month (under 1,000 email notifications, free tier).
    - Amazon EventBridge: $0.00/month (under 1 million events/month, free tier).

Total: $271.22/month

### 7. Risk Assessment
#### Risk Matrix
- Security vulnerabilities in Docker Image: Medium impact, medium probability.
- Database connection overload or exhaustion: High impact, medium probability.
- Cross-service permission errors (IAM Misconfiguration): High impact, low probability.

#### Mitigation Strategies
- Image vulnerabilities: Thoroughly handled via ECR's automatic Scan on push mechanism to detect malicious code immediately after Build phase completion.
- Connection overload: Completely controlled via centralized AWS RDS Proxy management layer (connection pooling) to cluster and reuse connections intelligently.
- Permission errors: Apply principle of least privilege, check Security Groups matrix and strictly configure specialized IAM Roles for each cloud service category from day one.

#### Contingency Plans
- Perform regular DR Drills: Simulate primary database failure to activate RDS Multi-AZ's automatic failover feature to Standby zone with target downtime under 60 seconds.
- Activate state recovery from automatic S3 Backup Bucket backups in case of serious data integrity incidents.

### 8. Expected Outcomes
#### Technical Improvements:
Successfully build a fully automated cloud infrastructure meeting real Production standards: automate secure 3-phase code handover pipeline, set up Multi-AZ High Availability network avoiding single points of failure, optimize data communication via Proxy and master centralized monitoring and proactive incident alerting before errors affect end-user experience.
#### Long-term Value
Bring a standardized blueprint document in terms of infrastructure architecture, practical DevOps and Platform Engineering processes for enterprises; serve as a solid secure infrastructure foundation for quick deployment of complex full-stack or microservices service systems later.
