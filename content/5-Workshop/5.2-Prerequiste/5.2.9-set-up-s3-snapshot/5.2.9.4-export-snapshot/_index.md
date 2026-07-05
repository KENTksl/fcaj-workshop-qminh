---
title: "Export Snapshot to S3"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.2.9.4. </b> "
---

## Objectives

Export an RDS snapshot to S3 for long-term storage or analysis when needed.

## Implementation steps

### Step 4.1 — Create IAM Role for RDS Export

1. Go to **IAM → Roles → Create role**.
2. Select **Trusted entity**: `Custom trust policy`.
3. Enter the following JSON:
   ```json
   {
     "Version": "2012-10-17",
     "Statement": [{
       "Effect": "Allow",
       "Principal": { "Service": "export.rds.amazonaws.com" },
       "Action": "sts:AssumeRole"
     }]
   }
   ```
4. Click **Next**.
5. Add inline policy:
   - Select **Create inline policy** → Select `JSON` tab → Enter the following JSON:
     ```json
     {
       "Version": "2012-10-17",
       "Statement": [{
         "Effect": "Allow",
         "Action": [
           "s3:PutObject*",
           "s3:ListBucket",
           "s3:GetBucketLocation"
         ],
         "Resource": [
           "arn:aws:s3:::globalmart-backup-ACCOUNT_ID",
           "arn:aws:s3:::globalmart-backup-ACCOUNT_ID/*"
         ]
       }]
     }
     ```
   - Replace `ACCOUNT_ID` with your AWS Account ID.
6. Enter **Policy name**: `globalmart-rds-export-policy` → Click **Create policy**.
7. Return to the role creation page, enter **Role name**: `globalmart-rds-export-role` → Click **Create role**.

### Step 4.2 — Export snapshot to S3

1. Go to **RDS → Snapshots → select globalmart-db-baseline**.
2. Click **Actions → Export to Amazon S3**.
3. Enter the following information:
   - **Export identifier**: `globalmart-export`.
   - **S3 destination**: `globalmart-backup-ACCOUNT_ID`.
   - **IAM role**: `globalmart-rds-export-role`.
   - **Encryption**: `aws/rds` (default).
4. Click **Export snapshot**.
5. Export runs in the background (~10-20 minutes depending on DB size), doesn't affect the running DB.
![Overview](/images/5-Workshop/Workshop-img/rds_export-1.jpg)
![Overview](/images/5-Workshop/Workshop-img/rds_export-2.jpg)
## Expected results

The snapshot is successfully exported to the S3 bucket and can be used for long-term storage or analysis.
