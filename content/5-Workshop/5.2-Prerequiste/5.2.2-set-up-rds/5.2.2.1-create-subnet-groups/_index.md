---
title: "Create DB Subnet Groups"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.2.2.1. </b> "
---

## Objectives

Create a DB Subnet Group to assign the proper subnets for RDS inside the VPC.

## Steps

1. Open **Aurora and RDS** in the AWS Management Console.
2. Go to **Subnet groups** → **Create DB subnet group**.
3. Configure:
   - Name: `rds-subnetgroup`
   - Description: `subnet group for rds`
   - VPC: select `globalmart-vpc`
   - Availability Zones: select 2 AZs (ap-southeast-1a and ap-southeast-1b)
   - Subnets: select the corresponding **private subnets** in both AZs

   ![DB Subnet Group Setting](/images/5-Workshop/5.2-Prerequisite/5.2.2-RDS/subnet_setting.png)

4. Click **Create**.

## Expected results

- DB subnet group `rds-subnetgroup` is ready to be selected when creating the RDS database (RDS will be placed in private subnets).
