---
title : "Security, optimization, and extension checks"
date : 2024-01-01
weight : 5
chapter : false
pre : " <b> 5.5 </b> "
---

## Objectives

This page is used to summarize advanced content that helps bring the workshop closer to a production-ready environment, including security audits, operational optimization, and testing of some important scenarios.

## Suggested content

1. **Least privilege audit**
   - Review IAM roles, task roles, and security groups.
   - Ensure each component only has permissions for its intended use.

2. **Enable and monitor security scans**
   - Verify ECR scan on push works for frontend and backend images.
   - If possible, describe how to handle images with vulnerability alerts.

3. **Add logs and tracing**
   - Consider adding VPC Flow Logs, ALB access logs, or API Gateway logs.
   - Ensure enough data is available for incident root cause analysis.

4. **Cost and performance optimization**
   - Review ECS task size, RDS configuration, backup cycle, and ECR/S3 lifecycle policies.
   - Evaluate whether to enable autoscaling or adjust alarm thresholds.

5. **Extension checks**
   - Try rolling out a new version through GitHub Actions.
   - Try service restart or task replacement on ECS.
   - If you have data, describe a failover scenario or recovery from backup.

## Expected results

This page helps you add post-deployment assessment, showing that the project not only stops at running but also aims for safe, cost-effective, and stable operations.

## Suggested images to add

- ECR scan results.
- IAM role audit or security group matrix.
- VPC Flow Logs, ALB logs, or API Gateway logs.
- Performance dashboard or cost chart if you have one.
