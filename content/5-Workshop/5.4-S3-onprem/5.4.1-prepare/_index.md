---
title : "Set up VPC, IAM, and security layer"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.4.1 </b> "
---

## Objectives

Create the foundational infrastructure layer for the entire system, including Multi-AZ VPC, subnets, route tables, NAT Gateway, security groups, and IAM roles for runtime components.

## Implementation steps

1. **Create VPC and subnets**
   - Create 1 main VPC for the project.
   - Create 2 public subnets and 2 private subnets evenly distributed across 2 Availability Zones.
   - Clearly mark which subnets are used for ALB, ECS services, and data layer.

2. **Configure Internet Gateway and NAT Gateway**
   - Attach Internet Gateway to VPC for public access.
   - Create 2 NAT Gateways, one per AZ to avoid SPOF.
   - Separate route tables for public and private subnets.

3. **Design security groups**
   - Create security groups for Public ALB, Internal ALB, ECS Frontend, ECS Backend, RDS Proxy, and RDS.
   - Only open necessary connections following least privilege principle.

4. **Create IAM roles**
   - Create execution role and task role for ECS.
   - Create roles for GitHub Actions, ECS, and monitoring components if needed.

## Expected results

After this step, you have a network and security framework ready to attach application and data components in their correct positions.

## Suggested images to add

- VPC and 4 subnets diagram.
- Route tables and 2 NAT Gateways.
- List of security groups.
- Main IAM roles.
