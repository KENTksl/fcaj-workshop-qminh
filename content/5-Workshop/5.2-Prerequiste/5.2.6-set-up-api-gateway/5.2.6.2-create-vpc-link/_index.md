---
title: "Create VPC Link"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.2.6.2. </b> "
---

## Objectives

Create a VPC Link so API Gateway can connect to internal resources inside the VPC.

## Steps

1. Open **API Gateway** in the AWS Management Console.
2. Go to **VPC links** → **Create**.
3. Configure:
   - VPC link version: **VPC link V2**
   - Name: `vpclink-globalmart`
   - VPC: `globalmart-vpc`
   - Subnets: select the **private subnets** created in the VPC step
   - Security groups: select `alb-internal-sg`

   ![VPC Link Setting](/images/5-Workshop/5.2-Prerequisite/5.2.6-APIGateway/vpclink_setting1.png)

4. Click **Create** and wait until it becomes available.

## Expected results

- VPC Link `vpclink-globalmart` is created successfully and can be used in the HTTP API integration.

  ![VPC Link Complete](/images/5-Workshop/5.2-Prerequisite/5.2.6-APIGateway/vpclink_complete.png)
