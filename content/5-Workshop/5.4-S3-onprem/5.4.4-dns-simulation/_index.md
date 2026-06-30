---
title : "Set up CloudWatch, Backup, and system testing"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.4.4 </b> "
---

## Objectives

Add operational observation, alerting, and backup layers so the system not only runs but is also easy to monitor, easy to detect incidents, and easy to recover when needed.

## Implementation steps

1. **Configure CloudWatch Logs**
   - Send logs from frontend and backend containers to CloudWatch Logs.
   - Separate log groups by component for easier tracing.

2. **Collect metrics and create dashboard**
   - Monitor ALB, ECS, RDS, CPU, memory, request count, latency, and important metrics.
   - Create a comprehensive CloudWatch Dashboard to monitor the entire system.

3. **Create alarms and SNS notifications**
   - Configure CloudWatch Alarms for high CPU, application errors, increased latency, abnormal RDS, or failed health checks.
   - Attach SNS topic to send alerts via email or appropriate notification channel.

4. **Configure backup and recovery**
   - Enable Automated Backup for RDS.
   - Prepare S3 backup bucket if you have snapshot export mechanism or backup file storage.
   - Define data recovery procedure when incidents occur.

5. **End-to-end testing**
   - Test accessing frontend from the Internet.
   - Test calling backend API through API Gateway.
   - Test database read/write operations through backend.
   - If possible, describe failover test or DR drill.

## Expected results

After this step, the system has basic monitoring, alerting, and backup capabilities to serve more stable operations in production environment.

## Suggested images to add

- CloudWatch Logs.
- Comprehensive dashboard.
- CloudWatch Alarms and SNS topic.
- Backup, snapshot, or end-to-end test results.
