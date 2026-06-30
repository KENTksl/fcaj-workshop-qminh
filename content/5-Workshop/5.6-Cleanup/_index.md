---
title : "Resource cleanup"
date : 2024-01-01
weight : 6
chapter : false
pre : " <b> 5.6 </b> "
---

## Objectives

After completing the workshop, a resource cleanup step is needed to avoid unnecessary costs and keep the environment tidy.

## Suggested cleanup order

1. Disable or pause GitHub Actions workflows if no longer deploying.
2. Reduce ECS service/task count or delete ECS cluster if workshop is completed.
3. Delete API Gateway, VPC Link, Public ALB, Internal ALB, and target groups.
4. Delete RDS Proxy, RDS instance, Secrets Manager secret, and unnecessary snapshots.
5. Delete ECR images/repositories if no longer needed.
6. Empty and delete S3 buckets used for artifacts or backups if this is a lab account.
7. Delete CloudWatch log groups, dashboards, alarms, and SNS topic if no longer used.
8. Delete NAT Gateway, route tables, subnets, security groups, and finally the VPC.

## Points to note

- Don't accidentally delete resources being used for other environments.
- Check billing after cleanup to ensure no high-cost resources remain like NAT Gateway, ALB, ECS, or RDS.
- If this is a long-term demo environment, you can just pause some components instead of deleting completely.

## Suggested images to add

- Resource list before deletion.
- Deleting ECS/ALB/RDS.
- Billing or resource list after cleanup.
