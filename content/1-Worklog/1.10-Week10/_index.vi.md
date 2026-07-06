---
title: "Worklog Tuần 10"
date: 2026-06-22
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Mục tiêu tuần 10:

- Thiết kế và triển khai hạ tầng mạng (VPC, Subnet, Security Group, NAT Gateway, IGW)
  cho project `GlobalMart` trên AWS.
- Triển khai cơ sở dữ liệu `RDS for MySQL` (Single-AZ) trong private subnet.
- Đóng gói ứng dụng `Frontend` và `Backend` thành Docker image,
  đẩy lên `Amazon ECR`.
- Thiết lập `Application Load Balancer` (internet-facing và internal)
  và triển khai `ECS Cluster` với `Fargate` cho cả 2 service.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                                                                                                                                                                                                                                                                                                                                                        | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------ | --------------- | -------------- |
| 2   | - Thiết kế kiến trúc tổng thể hệ thống `GlobalMart` trên AWS <br> - Tạo `VPC` (`globalmart-vpc`, CIDR `10.0.0.0/16`) <br>&emsp; + Tạo `Public subnet`, `Private subnet A` (ECS), `Private subnet B` (RDS) <br>&emsp; + Tạo `Internet Gateway (IGW)` và `NAT Gateway` <br>&emsp; + Cấu hình `Route Table` cho từng subnet                                                                                                                         | 22/06/2026   | 22/06/2026      |                |
| 3   | - Tạo các `Security Group` phân tách traffic theo từng tầng: <br>&emsp; + `sg-alb-public` cho ALB internet-facing <br>&emsp; + `sg-ecs-frontend` cho ECS Frontend task <br>&emsp; + `sg-ecs-backend` cho ECS Backend task <br>&emsp; + `sg-alb-internal` cho ALB internal <br>&emsp; + `sg-rds` cho RDS MySQL <br> - Tạo `RDS for MySQL` (Single-AZ, `db.t3.micro`) trong `Private subnet B`                                                     | 23/06/2026   | 23/06/2026      |                |
| 4   | - Tạo `ECR Repository` cho `globalmart-frontend` và `globalmart-backend` <br> - Build `Docker image` cho Frontend (`React/Vite`) và Backend (`Spring Boot`) <br> - Push image lên `Amazon ECR` <br> - Tạo database `globalmart` trong RDS instance qua EC2 tạm                                                                                                                                                                                   | 24/06/2026   | 24/06/2026      |                |
| 5   | - Tạo `Application Load Balancer` internet-facing (Public subnet) <br>&emsp; + Tạo `Target Group tg-frontend` (type IP, port 80) <br>&emsp; + Cấu hình Listener HTTP:80 → forward `tg-frontend` <br> - Tạo `Application Load Balancer` internal (Private subnet A) <br>&emsp; + Tạo `Target Group tg-backend` (type IP, port 8080) <br>&emsp; + Cấu hình Health check path `/actuator/health`                                                    | 25/06/2026   | 25/06/2026      |                |
| 6   | - Tạo `ECS Cluster` (`globalmart-cluster`, Fargate) <br> - Tạo `Task Definition` cho Frontend và Backend <br>&emsp; + Cấu hình CPU, Memory, Container port, Environment variables, CloudWatch Logs <br> - Tạo `ECS Service Frontend` gắn `ALB internet-facing` → `tg-frontend` <br> - Tạo `ECS Service Backend` gắn `ALB internal` → `tg-backend` <br> - Xử lý các lỗi phát sinh: Service-Linked Role, Health check grace period, Security Group | 26/06/2026   | 26/06/2026      |                |

### Kết quả đạt được tuần 10:

- Thiết kế và triển khai thành công hạ tầng mạng trên AWS:
  - `VPC globalmart-vpc` với CIDR `10.0.0.0/16`.
  - Phân chia đúng 3 tier: Public subnet (ALB, NAT), Private subnet A (ECS),
    Private subnet B (RDS).
  - `Internet Gateway` và `NAT Gateway` cấu hình đúng route table cho từng subnet.

- Triển khai thành công `RDS for MySQL` (Single-AZ) trong private subnet:
  - Không public access, chỉ cho phép kết nối từ `sg-ecs-backend` qua port 3306.
  - Tạo database `globalmart` và xác nhận Backend kết nối thành công.

- Đóng gói và push thành công Docker image lên `Amazon ECR`:
  - `globalmart-frontend` image cho React/Vite app.
  - `globalmart-backend` image cho Spring Boot API.

- Triển khai thành công `ECS Cluster` với `Fargate`:
  - `ECS Service Frontend` chạy ổn định, nhận traffic qua ALB internet-facing.
  - `ECS Service Backend` kết nối được RDS, xử lý API qua ALB internal.
  - Cấu hình đúng Health check (`/actuator/health`), Health check grace period
    (`120 giây`) để tránh vòng lặp restart.

- Nắm được nguyên lý phân tách traffic theo Security Group:
  - ALB public → ECS Frontend → ALB internal → ECS Backend → RDS.
  - Mỗi tầng chỉ cho phép đúng nguồn traffic cần thiết, không mở rộng thừa.
