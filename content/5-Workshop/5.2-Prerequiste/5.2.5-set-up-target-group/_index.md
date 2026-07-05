---
title: "Set up Target Group"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.2.5. </b> "
---

## Objectives

Create target groups to connect the load balancer with ECS services or other backend targets.

## Steps

1. Open the **EC2 Dashboard** in the AWS Management Console.
2. Go to **Target Groups** → **Create target group**.

3. Create target group `tg-frontend`:
   - Target type: **IP addresses**
   - Target group name: `tg-frontend`
   - Protocol: HTTP
   - Port: 80
   - VPC: `globalmart-vpc`

   ![TG Setting 1](/images/5-Workshop/5.2-Prerequisite/5.2.5-TG/tg_setting1.png)
   - Health check protocol: HTTP
   - Health check path: `/`
   - Target optimizer: Off (Default)

   ![TG Setting 2](/images/5-Workshop/5.2-Prerequisite/5.2.5-TG/tg_setting2.png)

4. Create target group `tg-backend` with the same settings, but:
   - Port: 8080
   - Health check path: `/actuator/health`

## Expected results

- You have 2 Target Groups `tg-frontend` and `tg-backend` ready for ALB listeners and ECS services.

  ![TG Completed](/images/5-Workshop/5.2-Prerequisite/5.2.5-TG/tg_complete.png)
