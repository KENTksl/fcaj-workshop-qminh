---
title: "Security, optimization, and extension checks"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

## Objectives

This page is used to summarize advanced content that helps bring the workshop closer to a production-ready environment, including security audits, operational optimization, and testing of some important scenarios.

## Main content

Section `5.4` is organized by the following advanced content groups:

1. **Least privilege audit** - Review IAM roles, task roles, and security groups, ensure each component only has permissions for its intended use.
2. **Enable and monitor security scans** - Verify ECR scan on push works for frontend and backend images, describe how to handle images with vulnerability alerts.
3. **Add logs and tracing** - Consider adding VPC Flow Logs, ALB access logs, or API Gateway logs, ensure enough data is available for incident root cause analysis.
4. **Cost and performance optimization** - Review ECS task size, RDS configuration, backup cycle, and ECR/S3 lifecycle policies, evaluate whether to enable autoscaling or adjust alarm thresholds.
5. **Extension checks** - Try rolling out a new version through GitHub Actions, try service restart or task replacement on ECS, describe a failover scenario or recovery from backup.

## Expected results

After this section, you have added post-deployment assessment, showing that the project not only stops at running but also aims for safe, cost-effective, and stable operations.

## Suggested images to add

- ECR scan results.
- IAM role audit or security group matrix.
- VPC Flow Logs, ALB logs, or API Gateway logs.
- Performance dashboard or cost chart if you have one.
