---
title: "Thiết lập Load Balancers"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.2.4. </b> "
---

## Mục tiêu

Chuẩn bị các load balancer cần thiết để phân phối lưu lượng giữa frontend, backend và các thành phần tích hợp.

## Các bước thực hiện

1. Truy cập **EC2 Dashboard** trên AWS Management Console.
2. Chọn **Load Balancers** → **Create load balancer**.
3. Chọn **Application Load Balancer (ALB)**.

   ![ALB Setting 1](/images/5-Workshop/5.2-Prerequisite/5.2.4-ALB/alb_setting1.png)

4. Tạo **Public ALB** (internet-facing):
   - Load balancer name: `alb-internet-facing`
   - Scheme: **Internet-facing**

   ![ALB Setting 2](/images/5-Workshop/5.2-Prerequisite/5.2.4-ALB/alb_setting2.png)
   - VPC: chọn `globalmart-vpc`
   - Availability Zones/Subnets: chọn **2 public subnets** (ap-southeast-1a và ap-southeast-1b)

   ![ALB Setting 3](/images/5-Workshop/5.2-Prerequisite/5.2.4-ALB/alb_setting3.png)
   - Security group: chọn `alb-public-sg`

   ![ALB Setting 4](/images/5-Workshop/5.2-Prerequisite/5.2.4-ALB/alb_setting4.png)
   - Listener: HTTP : 80
   - Default action: forward tới Target Group `tg-frontend` (tạo ở 5.2.5)

   ![ALB Setting 5](/images/5-Workshop/5.2-Prerequisite/5.2.4-ALB/alb_setting5.png)

5. Tạo **Internal ALB** (internal):
   - Load balancer name: `alb-internal`
   - Scheme: **Internal**
   - VPC/subnets: chọn các subnet trong VPC theo nhu cầu (giữ giống cấu trúc 2 AZ)
   - Security group: `alb-internal-sg`
   - Listener: HTTP : 80
   - Default action: forward tới Target Group `tg-backend` (tạo ở 5.2.5)

## Kết quả mong đợi

- Bạn có 2 ALB: `alb-internet-facing` và `alb-internal` (đều Active).

  ![ALB Completed](/images/5-Workshop/5.2-Prerequisite/5.2.4-ALB/alb_complete.png)
