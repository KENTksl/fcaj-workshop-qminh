---
title: "Create Security Group"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.2.1.2. </b> "
---

## Objectives

Create the base Security Groups to control traffic between components inside the VPC (ALB, ECS, and RDS).

## Steps

1. Open the **VPC Dashboard** and select **Security Groups**.
2. Click **Create security group** and create the Security Groups listed below.

   ![SG Create](/images/5-Workshop/5.2-Prerequisite/5.2.1-VPC/sg_create.png)

3. For each Security Group:
   - Select the correct VPC (**globalmart-vpc**)
   - Outbound rules: keep the default **All traffic** to `0.0.0.0/0` (as shown)
   - Inbound rules: configure per group below

4. Create 5 Security Groups:
   - `alb-public-sg` (ALB internet-facing)
     - Inbound: HTTP 80 from `0.0.0.0/0`
     - Inbound: HTTPS 443 from `0.0.0.0/0`
   - `alb-internal-sg` (ALB internal)
     - Inbound: HTTP 80 from `0.0.0.0/0`
   - `ecs-frontend-sg` (ECS frontend task)
     - Inbound: TCP 80 from `alb-public-sg`
   - `ecs-backend-sg` (ECS backend task)
     - Inbound: TCP 8080 from `alb-internal-sg`
   - `rds-sg` (RDS MySQL)
     - Inbound: MySQL/Aurora 3306 from `ecs-backend-sg`

5. After creation, verify the Security Groups list.

   ![SG Complete](/images/5-Workshop/5.2-Prerequisite/5.2.1-VPC/sg_complete.png)

## Expected results

- You have the Security Groups required for public ALB, internal ALB, ECS frontend/backend, and RDS for the next steps.
