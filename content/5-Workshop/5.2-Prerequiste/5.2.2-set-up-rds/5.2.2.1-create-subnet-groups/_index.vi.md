---
title: "Tạo DB Subnet Groups"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.2.2.1. </b> "
---

## Mục tiêu

Tạo DB Subnet Group để gán các subnet phù hợp cho RDS trong VPC.

## Các bước thực hiện

1. Truy cập **Aurora and RDS** trên AWS Management Console.
2. Chọn **Subnet groups** → **Create DB subnet group**.
3. Cấu hình như sau:
   - Name: `rds-subnetgroup`
   - Description: `subnet group for rds`
   - VPC: chọn VPC đã tạo (`globalmart-vpc`)
   - Availability Zones: chọn 2 AZ (ap-southeast-1a và ap-southeast-1b)
   - Subnets: chọn các **private subnets** tương ứng ở 2 AZ

   ![DB Subnet Group Setting](/images/5-Workshop/5.2-Prerequisite/5.2.2-RDS/subnet_setting.png)

4. Bấm **Create** để tạo DB subnet group.

## Kết quả mong đợi

- DB subnet group `rds-subnetgroup` sẵn sàng để chọn ở bước tạo database (RDS sẽ được đặt trong các private subnets).
