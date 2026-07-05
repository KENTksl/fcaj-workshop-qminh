---
title: "Tạo Security Group"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.2.1.2. </b> "
---

## Mục tiêu

Tạo các Security Group cơ bản để kiểm soát luồng truy cập giữa các thành phần trong VPC (ALB, ECS và RDS).

## Các bước thực hiện

1. Truy cập **VPC Dashboard** và chọn **Security Groups**.
2. Bấm **Create security group** và cấu hình lần lượt các Security Group theo danh sách bên dưới.

   ![SG Create](/images/5-Workshop/5.2-Prerequisite/5.2.1-VPC/sg_create.png)

3. Với mỗi Security Group:
   - Chọn đúng VPC (**globalmart-vpc**)
   - Outbound rules: giữ mặc định **All traffic** ra `0.0.0.0/0` (như hình)
   - Inbound rules: cấu hình theo từng nhóm dưới đây

4. Tạo 5 Security Group:
   - `alb-public-sg` (ALB internet-facing)
     - Inbound: HTTP 80 từ `0.0.0.0/0`
     - Inbound: HTTPS 443 từ `0.0.0.0/0`
   - `alb-internal-sg` (ALB internal)
     - Inbound: HTTP 80 từ `0.0.0.0/0`
   - `ecs-frontend-sg` (ECS frontend task)
     - Inbound: TCP 80 từ `alb-public-sg`
   - `ecs-backend-sg` (ECS backend task)
     - Inbound: TCP 8080 từ `alb-internal-sg`
   - `rds-sg` (RDS MySQL)
     - Inbound: MySQL/Aurora 3306 từ `ecs-backend-sg`

5. Sau khi tạo xong, kiểm tra danh sách Security Group đã có đủ các nhóm như hình.

   ![SG Complete](/images/5-Workshop/5.2-Prerequisite/5.2.1-VPC/sg_complete.png)

## Kết quả mong đợi

- Bạn có đủ các Security Group cho public ALB, internal ALB, ECS frontend/backend và RDS để dùng xuyên suốt các bước tiếp theo.
