---
title: "Set up Load Balancers"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.2.4. </b> "
---

## Objectives

Prepare the required load balancers to distribute traffic between frontend, backend, and integration components.

## Steps

1. Open the **EC2 Dashboard** in the AWS Management Console.
2. Go to **Load Balancers** → **Create load balancer**.
3. Choose **Application Load Balancer (ALB)**.

   ![ALB Setting 1](/images/5-Workshop/5.2-Prerequisite/5.2.4-ALB/alb_setting1.png)

4. Create the **Public ALB** (internet-facing):
   - Load balancer name: `alb-internet-facing`
   - Scheme: **Internet-facing**

   ![ALB Setting 2](/images/5-Workshop/5.2-Prerequisite/5.2.4-ALB/alb_setting2.png)
   - VPC: `globalmart-vpc`
   - Availability Zones/Subnets: select **2 public subnets** (ap-southeast-1a and ap-southeast-1b)

   ![ALB Setting 3](/images/5-Workshop/5.2-Prerequisite/5.2.4-ALB/alb_setting3.png)
   - Security group: select `alb-public-sg`

   ![ALB Setting 4](/images/5-Workshop/5.2-Prerequisite/5.2.4-ALB/alb_setting4.png)
   - Listener: HTTP : 80
   - Default action: forward to Target Group `tg-frontend` (created in 5.2.5)

   ![ALB Setting 5](/images/5-Workshop/5.2-Prerequisite/5.2.4-ALB/alb_setting5.png)

5. Create the **Internal ALB**:
   - Load balancer name: `alb-internal`
   - Scheme: **Internal**
   - VPC/subnets: select the subnets in your VPC (keep the same 2-AZ structure)
   - Security group: `alb-internal-sg`
   - Listener: HTTP : 80
   - Default action: forward to Target Group `tg-backend` (created in 5.2.5)

## Expected results

- You have 2 ALBs: `alb-internet-facing` and `alb-internal` (both Active).

  ![ALB Completed](/images/5-Workshop/5.2-Prerequisite/5.2.4-ALB/alb_complete.png)
