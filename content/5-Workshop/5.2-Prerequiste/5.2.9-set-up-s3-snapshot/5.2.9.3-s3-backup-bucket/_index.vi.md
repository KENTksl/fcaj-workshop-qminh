---
title: "S3 Backup Bucket"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.2.9.3. </b> "
---

## Mục tiêu

Tạo S3 bucket để lưu trữ các file backup dài hạn, cấu hình lifecycle rule để tiết kiệm chi phí.

## Các bước thực hiện

### 1. Tạo S3 Backup Bucket

1. Vào **S3 → Create bucket**.
2. Nhập các thông tin:
   - **Bucket name**: `globalmart-backup-ACCOUNT-ID`.
   - **AWS Region**: `ap-southeast-1`.
   - **Block all public access**: ON (tất cả).
   - **Bucket Versioning**: Chọn `Enable`.
3. Bấm **Create bucket**.
![Overview](/images/5-Workshop/Workshop-img/s3_bucket-1.jpg)
![Overview](/images/5-Workshop/Workshop-img/s3_bucket-2.jpg)
### 2. Thêm Lifecycle rule để tiết kiệm chi phí

1. Vào **S3 → globalmart-backup-ACCOUNT-ID → tab Management**.
2. Chọn **Lifecycle rules → Create lifecycle rule**.
3. Nhập thông tin:
   - **Rule name**: `backup-lifecycle`.
   - **Rule scope**: Chọn `Apply to all objects`.
   - **Transition actions**:
     - Check `Transition current versions of objects` → Chọn `S3 Glacier Flexible Retrieval` → Sau `30 days`.
   - **Expiration actions**:
     - Check `Expire current versions of objects` → Sau `365 days`.
4. Bấm **Create rule**.
![Overview](/images/5-Workshop/Workshop-img/life_cycle-1.jpg)
![Overview](/images/5-Workshop/Workshop-img/life_cycle-2.jpg)
![Overview](/images/5-Workshop/Workshop-img/life_cycle-3.jpg)
## Kết quả mong đợi

Objects mới lưu 30 ngày đầu ở S3 Standard (truy cập nhanh), sau đó tự chuyển sang Glacier (rẻ hơn ~80%), xóa sau 1 năm.
