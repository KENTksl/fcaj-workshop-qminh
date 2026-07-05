---
title: "Tạo VPC"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.2.1.1. </b> "
---

## Mục tiêu

Tạo một VPC làm nền tảng mạng chính cho toàn bộ hệ thống GlobalMart trên AWS (bao gồm public/private subnets và NAT gateway).

## Các bước thực hiện

1. Truy cập **VPC Dashboard** trên AWS Management Console.
2. Chọn **Create VPC**.
3. Ở phần **VPC settings**:
   - Resources to create: **VPC and more**
   - Name tag auto-generation: **globalmart**
   - IPv4 CIDR block: **10.0.0.0/16**
   - IPv6 CIDR block: **No IPv6 CIDR block**
   - Number of Availability Zones (AZs): **2**

   ![VPC Setting 1](/images/5-Workshop/5.2-Prerequisite/5.2.1-VPC/vpc_setting1.png)
4. Ở phần subnet/NAT/DNS:
   - Number of public subnets: **2**
   - Number of private subnets: **2**
   - NAT gateways: **Zonal** (Number of NAT gateways: **1 in 1 AZ**)
   - VPC endpoints: **None**
   - DNS options: **Enable DNS hostnames** và **Enable DNS resolution**

   ![VPC Setting 2](/images/5-Workshop/5.2-Prerequisite/5.2.1-VPC/vpc_setting2.png)
5. Bấm **Create VPC** và chờ tạo xong. Kiểm tra lại VPC vừa tạo (trạng thái hoạt động, `VPC ID`, `CIDR block`).

   ![VPC Completed](/images/5-Workshop/5.2-Prerequisite/5.2.1-VPC/create_complete.png)

## Kết quả mong đợi

- Bạn có một VPC (globalmart-vpc) với 2 public subnets + 2 private subnets và NAT gateway để dùng cho các bước RDS/ALB/ECS phía sau.
