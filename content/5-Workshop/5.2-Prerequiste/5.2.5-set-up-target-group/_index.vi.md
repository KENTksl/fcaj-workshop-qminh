---
title: "Thiết lập Target Group"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.2.5. </b> "
---

## Mục tiêu

Tạo target groups để liên kết load balancer với ECS services hoặc backend targets tương ứng.

## Các bước thực hiện

1. Truy cập **EC2 Dashboard** trên AWS Management Console.
2. Chọn **Target Groups**
3. Chọn **Create target group**.

4. Tạo target group `tg-frontend`:
   - Target type: **IP addresses**
   - Target group name: `tg-frontend`
   - Protocol: HTTP
   - Port: 80
   - VPC: `globalmart-vpc`

   ![TG Setting 1](/images/5-Workshop/5.2-Prerequisite/5.2.5-TG/tg_setting1.png)
   - Health check protocol: HTTP
   - Health check path: `/`
   - Target optimizer: Off (Default)

   ![TG Setting 2](/images/5-Workshop/5.2-Prerequisite/5.2.5-TG/tg_setting2.png)

5. Tạo target group `tg-backend` với cấu hình tương tự, thay đổi:
   - Target group name: `tg-backend`
   - Port: 8080
   - Health check path: `/actuator/health`

## Kết quả mong đợi

- Bạn có 2 Target Groups `tg-frontend` và `tg-backend` để gán cho ALB và ECS services.

  ![TG Completed](/images/5-Workshop/5.2-Prerequisite/5.2.5-TG/tg_complete.png)
