---
title: "S3 Backup Bucket"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.2.9.3. </b> "
---

## Objectives

Create an S3 bucket to store long-term backup files, configure a lifecycle rule to save costs.

## Implementation steps

### 1. Create S3 Backup Bucket

1. Go to **S3 → Create bucket**.
2. Enter the following information:
   - **Bucket name**: `globalmart-backup-ACCOUNT-ID`.
   - **AWS Region**: `ap-southeast-1`.
   - **Block all public access**: ON (all).
   - **Bucket Versioning**: Select `Enable`.
3. Click **Create bucket**.
![Overview](/images/5-Workshop/Workshop-img/s3_bucket-1.jpg)
![Overview](/images/5-Workshop/Workshop-img/s3_bucket-2.jpg)
### 2. Add Lifecycle rule to save costs

1. Go to **S3 → globalmart-backup-ACCOUNT-ID → Management tab**.
2. Select **Lifecycle rules → Create lifecycle rule**.
3. Enter information:
   - **Rule name**: `backup-lifecycle`.
   - **Rule scope**: Select `Apply to all objects`.
   - **Transition actions**:
     - Check `Transition current versions of objects` → Select `S3 Glacier Flexible Retrieval` → After `30 days`.
   - **Expiration actions**:
     - Check `Expire current versions of objects` → After `365 days`.
4. Click **Create rule**.
![Overview](/images/5-Workshop/Workshop-img/life_cycle-1.jpg)
![Overview](/images/5-Workshop/Workshop-img/life_cycle-2.jpg)
![Overview](/images/5-Workshop/Workshop-img/life_cycle-3.jpg)
## Expected results

New objects are stored in S3 Standard for the first 30 days (fast access), then automatically transition to Glacier (about 80% cheaper), and deleted after 1 year.
