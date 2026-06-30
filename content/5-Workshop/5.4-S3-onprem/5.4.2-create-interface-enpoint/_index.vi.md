---
title : "Triển khai ECS, ALB, API Gateway và VPC Link"
date : 2024-01-01
weight : 2
chapter : false
pre : " <b> 5.4.2 </b> "
---

## Mục tiêu

Đưa frontend và backend vào môi trường chạy, đồng thời cấu hình các lớp tiếp nhận và định tuyến request theo đúng kiến trúc dự án.

## Các bước thực hiện

1. **Tạo ECS Cluster và task definitions**
- Tạo ECS Cluster sử dụng AWS Fargate.
- Tạo task definition riêng cho frontend và backend.
- Cấu hình CPU, memory, port mapping, logging và các biến môi trường cơ bản.

2. **Triển khai ECS services**
- Tạo service cho frontend và backend trong private subnets.
- Cấu hình desired count và autoscaling nếu bạn dùng trong dự án.

3. **Tạo Public ALB**
- Tạo Internet-facing ALB để tiếp nhận request từ bên ngoài.
- Gắn target group phù hợp cho frontend service.
- Cấu hình listener, health check và rule điều hướng nếu cần.

4. **Tạo Internal ALB**
- Tạo Internal ALB để phân phối request đến backend service trong VPC.
- Gắn backend target group và cấu hình listener nội bộ.

5. **Tạo API Gateway và VPC Link**
- Tạo API Gateway làm lớp vào cho backend.
- Tạo VPC Link để kết nối API Gateway với Internal ALB.
- Cấu hình route `/api` hoặc các route nghiệp vụ phù hợp với ứng dụng của bạn.

## Luồng truy cập mong muốn

- Người dùng truy cập hệ thống qua Public ALB.
- Frontend được phân phối từ ECS service phía trước.
- Frontend hoặc client gọi API qua API Gateway.
- API Gateway dùng VPC Link để vào Internal ALB.
- Internal ALB chuyển tiếp request tới backend ECS service.

## Kết quả mong đợi

Sau bước này, frontend và backend đều đã chạy trên ECS và có đường đi request rõ ràng từ Internet vào đến lớp xử lý nghiệp vụ.

## Gợi ý hình ảnh cần thêm

- Hình ECS Cluster và ECS services.
- Hình task definition của frontend và backend.
- Hình Public ALB và target group.
- Hình API Gateway, VPC Link và Internal ALB.
