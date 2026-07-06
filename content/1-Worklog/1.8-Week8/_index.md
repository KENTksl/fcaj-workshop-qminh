---
title: "Week 8 Worklog"
date: 2026-06-08
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Week 8 Objectives:

- Finalize the overall architecture model for the final project to prepare for the actual deployment phase.
- Clearly decompose the main components of the system including CI/CD, containerization, application runtime, database, backup, and monitoring.
- Align on the project deployment direction on AWS utilizing ECS Fargate, RDS, CloudWatch, and related services.

### Tasks to be Deployed This Week:

| Day | Tasks                                                                                                                                                                                                                                                                                                                                                                                                                          | Start Date | End Date   | Reference |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | ---------- | --------- |
| 2   | - Analyze final project requirements and identify the main components of the system <br>&emsp; + Determine the separate frontend/backend deployment model <br>&emsp; + Define the containerization approach to package applications <br>&emsp; + Brainstorm the overall architecture on AWS to serve the project implementation phase                                                                                          | 08/06/2026 | 08/06/2026 | -         |
| 3   | - Design the **Build & Containerization Flow** block <br>&emsp; + Define the workflow from GitHub to CodePipeline <br>&emsp; + Assign roles for CodeBuild, CodeDeploy, ECR, and Artifact Bucket <br>&emsp; + Align on the image building process, storing images on ECR, and preparing for the deployment step                                                                                                                 | 09/06/2026 | 09/06/2026 | -         |
| 4   | - Design the **Deployment (Application Runtime)** block <br>&emsp; + Determine VPC, Internet Gateway, Public Subnets, and Private Subnets for the system <br>&emsp; + Develop the ECS Cluster deployment model on ECS Fargate <br>&emsp; + Separate into 2 services: Frontend and Backend <br>&emsp; + Design internet-facing ALB and internal ALB for access workflow between application tiers                               | 10/06/2026 | 10/06/2026 | -         |
| 5   | - Design the **Database, Recovery & Backup** block <br>&emsp; + Select Amazon RDS for MySQL as the database for the system <br>&emsp; + Determine DB placement in private subnets and the connection method from the backend <br>&emsp; + Supplement RDS Snapshot, backup bucket, and snapshot export plans to support backup/recovery                                                                                         | 11/06/2026 | 11/06/2026 | -         |
| 6   | - Design the **Monitoring & Observability** block and finalize the overall diagram <br>&emsp; + Define CloudWatch Logs, CloudWatch Metrics, and CloudWatch Alarms for the system <br>&emsp; + Identify SNS to send alerts via Email/SMS <br>&emsp; + Review all connections between CI/CD, ECS, RDS, backup, and monitoring <br>&emsp; + Finalize the architecture model to prepare for the final project implementation phase | 12/06/2026 | 12/06/2026 | -         |

### Week 8 Achievements:

- Finalized the overall architecture model for the final project on AWS:
  - Defined the **Build & Containerization Flow** block with GitHub, CodePipeline, CodeBuild, CodeDeploy, ECR, and Artifact Bucket.
  - Defined the **Deployment** block with VPC, Internet Gateway, Public Subnet, Private Subnet, ECS Cluster, ECS Fargate, Frontend, Backend, internet-facing ALB, and internal ALB.
  - Defined the **Database / Recovery & Backup** block with Amazon RDS for MySQL, RDS Snapshot, and Backup Bucket.
  - Defined the **Monitoring & Observability** block with CloudWatch Logs, CloudWatch Metrics, CloudWatch, SNS, and Email/SMS notifications.
- Agreed on the expected deployment workflow for the project:
  - Source code is managed on GitHub.
  - The pipeline handles building, artifact storage, and pushing container images to Amazon ECR.
  - The application is planned to run on ECS Fargate under a decoupled Frontend and Backend model.
  - The backend connects to RDS for MySQL in a private subnet.
- Clarified the backup and monitoring approach for the system before entering the hands-on implementation phase:
  - Backup is handled through RDS Snapshots and export to a backup bucket.
  - Logging, metrics, and alerting are monitored through CloudWatch and SNS.
- Prepared the design foundation for the team to move into the final project implementation stage in the following weeks.

#### Proposed Architecture Model for the Final Project

![GlobalMart architecture model](/images/2-Proposal/globalmart.png)
