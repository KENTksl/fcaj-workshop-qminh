---
title: "RDS Automated Backup"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.2.9.1. </b> "
---

## Mục tiêu

Thiết lập RDS Automated Backup để tự động tạo snapshot hàng ngày, giữ lại trong 7 ngày.

## Các bước thực hiện

1. Vào **RDS → Databases → globalmart-db → Modify**.
2. Kéo xuống phần **Additional configuration**:
   - **Backup retention period**: Chọn `7 days`.
   - **Backup window**: Chọn `18:00 - 19:00 UTC` (tương đương 01:00-02:00 SA giờ Việt Nam) 
3. Bấm **Continue → Apply immediately → Modify DB instance**.
4. Đợi vài giây, RDS Status quay về **Available**.
![Overview](/images/5-Workshop/Workshop-img/rds_auto-1.jpg)
### Kiểm tra backup đã bật

1. Vào **RDS → Databases → globalmart-db → tab Maintenance & backups**.
2. Xem phần "Automated backups" → Phải thấy: `Backup retention: 7 days`.
![Overview](/images/5-Workshop/Workshop-img/rds_auto-2.jpg)
### Xem các snapshots đã có

1. Vào **RDS → Automated backups → chọn globalmart-db**.
2. Thấy danh sách snapshots theo từng ngày.
![Overview](/images/5-Workshop/Workshop-img/rds_auto-3.jpg)
## Kết quả mong đợi

Mỗi ngày lúc 01:00 SA giờ Việt Nam, RDS tự tạo 1 snapshot. Giữ lại 7 ngày gần nhất, tự động xóa cái cũ hơn.
