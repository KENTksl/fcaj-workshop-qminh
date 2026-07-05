---
title: "Create VPC"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.2.1.1. </b> "
---

## Objectives

Create a VPC as the main network foundation for the entire GlobalMart system on AWS (including public/private subnets and a NAT gateway).

## Steps

1. Open the **VPC Dashboard** in the AWS Management Console.
2. Choose **Create VPC**.
3. In **VPC settings**:
   - Resources to create: **VPC and more**
   - Name tag auto-generation: **globalmart**
   - IPv4 CIDR block: **10.0.0.0/16**
   - IPv6 CIDR block: **No IPv6 CIDR block**
   - Number of Availability Zones (AZs): **2**

   ![VPC Setting 1](/images/5-Workshop/5.2-Prerequisite/5.2.1-VPC/vpc_setting1.png)

4. In subnet/NAT/DNS:
   - Number of public subnets: **2**
   - Number of private subnets: **2**
   - NAT gateways: **Zonal** (Number of NAT gateways: **1 in 1 AZ**)
   - VPC endpoints: **None**
   - DNS options: enable **DNS hostnames** and **DNS resolution**

   ![VPC Setting 2](/images/5-Workshop/5.2-Prerequisite/5.2.1-VPC/vpc_setting2.png)

5. Click **Create VPC** and wait until it is created. Verify the VPC basic information (`VPC ID`, `CIDR block`, status).

   ![VPC Completed](/images/5-Workshop/5.2-Prerequisite/5.2.1-VPC/create_complete.png)

## Expected results

- You have a VPC (globalmart-vpc) with 2 public subnets + 2 private subnets and a NAT gateway for the next steps (RDS/ALB/ECS).
