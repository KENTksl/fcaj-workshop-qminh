---
title: "Tạo Databases"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.2.2.2. </b> "
---

## Mục tiêu

Khởi tạo database trên Amazon RDS để phục vụ backend của hệ thống.

## Các bước thực hiện

1. Truy cập **RDS Dashboard** trên AWS Management Console.
2. Chọn **Create database** và cấu hình theo các bước dưới đây.

3. Engine + template:
   - Engine type: **MySQL**
   - Database creation method: **Full configuration**
   - Templates: **Dev/Test**

   ![RDS Setting 1](/images/5-Workshop/5.2-Prerequisite/5.2.2-RDS/rds_setting1.png)

4. Availability:
   - Deployment options: **Single-AZ DB instance deployment (1 instance)**

   ![RDS Setting 2](/images/5-Workshop/5.2-Prerequisite/5.2.2-RDS/rds_setting2.png)

5. DB identifier + credentials:
   - DB instance identifier: `db-globalmart`
   - Master username: `admin`
   - Credentials management: **Self managed**
   - Master password: tự đặt (không ghi password vào tài liệu)

   ![RDS Setting 3](/images/5-Workshop/5.2-Prerequisite/5.2.2-RDS/rds_setting3.png)

6. Instance configuration:
   - DB instance class: `db.m5.large`

   ![RDS Setting 4](/images/5-Workshop/5.2-Prerequisite/5.2.2-RDS/rds_setting4.png)

7. Connectivity:
   - Don't connect to an EC2 compute resource
   - Virtual private cloud (VPC): `globalmart-vpc`
   - DB subnet group: `rds-subnetgroup`
   - Public access: **No**
   - VPC security group: chọn **rds-sg**

   ![RDS Setting 5](/images/5-Workshop/5.2-Prerequisite/5.2.2-RDS/rds_setting5.png)
   ![RDS Setting 6](/images/5-Workshop/5.2-Prerequisite/5.2.2-RDS/rds_setting6.png)

8. Monitoring:
   - Database Insights: **Standard**
   - Enable **Performance Insights** (retention: 7 days)
   - Enable **Enhanced monitoring** (granularity: 60 seconds, role: default)

   ![RDS Setting 7](/images/5-Workshop/5.2-Prerequisite/5.2.2-RDS/rds_setting7.png)
   ![RDS Setting 8](/images/5-Workshop/5.2-Prerequisite/5.2.2-RDS/rds_setting8.png)

9. Bấm **Create database** và chờ trạng thái **Available**.

## Kết quả mong đợi

- Database `db-globalmart` được tạo thành công và sẵn sàng để backend kết nối.

  ![RDS Completed](/images/5-Workshop/5.2-Prerequisite/5.2.2-RDS/rds_complete.png)
