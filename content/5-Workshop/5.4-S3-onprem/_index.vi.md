---
title : "Triển khai runtime, data và networking"
date : 2024-01-01 
weight : 4 
chapter : false
pre : " <b> 5.4. </b> "
---

## Mục tiêu

Phần này tập trung vào lớp hạ tầng chạy ứng dụng, bao gồm VPC, ECS Fargate, ALB, API Gateway, VPC Link, tầng dữ liệu và các thành phần quan sát hệ thống.

## Nội dung

- [Thiết lập VPC, IAM và lớp bảo mật](5.4.1-prepare/)
- [Triển khai ECS, ALB, API Gateway và VPC Link](5.4.2-create-interface-enpoint/)
- [Cấu hình RDS Multi-AZ, Secrets Manager và RDS Proxy](5.4.3-test-endpoint/)
- [Thiết lập CloudWatch, Backup và kiểm thử hệ thống](5.4.4-dns-simulation/)

## Kết quả mong đợi

Sau phần này, bạn có một hạ tầng có thể tiếp nhận request từ Internet, điều phối lưu lượng nội bộ, kết nối dữ liệu an toàn và sẵn sàng cho việc giám sát trong vận hành.
