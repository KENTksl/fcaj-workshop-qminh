---
title: "Export Snapshot lên S3"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.2.9.4. </b> "
---

## Mục tiêu

Export RDS snapshot lên S3 để lưu trữ dài hạn hoặc phân tích khi cần.

## Các bước thực hiện

### Bước 4.1 — Tạo IAM Role cho RDS Export

1. Vào **IAM → Roles → Create role**.
2. Chọn **Trusted entity**: `Custom trust policy`.
3. Nhập JSON sau:
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
4. Bấm **Next**.
5. Thêm inline policy:
   - Chọn **Create inline policy** → Chọn tab `JSON` → Nhập JSON sau:
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
   - Thay `ACCOUNT_ID` bằng AWS Account ID của bạn.
6. Đặt **Policy name**: `globalmart-rds-export-policy` → Bấm **Create policy**.
7. Quay lại trang tạo role, đặt **Role name**: `globalmart-rds-export-role` → Bấm **Create role**.

### Bước 4.2 — Export snapshot lên S3

1. Vào **RDS → Snapshots → chọn globalmart-db-baseline**.
2. Bấm **Actions → Export to Amazon S3**.
3. Nhập các thông tin:
   - **Export identifier**: `globalmart-export`.
   - **S3 destination**: `globalmart-backup-ACCOUNT_ID`.
   - **IAM role**: `globalmart-rds-export-role`.
   - **Encryption**: `aws/rds` (default).
4. Bấm **Export snapshot**.
5. Export chạy nền (~10-20 phút tùy size DB), không ảnh hưởng DB đang chạy.
![Overview](/images/5-Workshop/Workshop-img/rds_export-1.jpg)
![Overview](/images/5-Workshop/Workshop-img/rds_export-2.jpg)
## Kết quả mong đợi

Snapshot được export thành công lên S3 bucket, có thể dùng để lưu trữ dài hạn hoặc phân tích.
