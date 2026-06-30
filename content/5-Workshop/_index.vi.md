---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Workshop triển khai hạ tầng GlobalMart trên AWS

#### Tổng quan

Phần này được dùng như bộ khung hướng dẫn triển khai cho dự án **GlobalMart** dựa trên sơ đồ kiến trúc và nội dung trong phần `Proposal`. Trọng tâm của workshop là xây dựng hạ tầng DevOps/Platform Engineering theo hướng production-ready, bao gồm luồng CI/CD, lớp mạng Multi-AZ, runtime ECS, tầng dữ liệu an toàn, giám sát tập trung và cơ chế sao lưu - cảnh báo.

Trong mỗi mục, tôi đã tạo sẵn khuôn nội dung theo dự án của bạn để bạn có thể tự bổ sung hình ảnh, thông số thực tế và kết quả triển khai sau này.

#### Phạm vi workshop

- Chuẩn bị source code, IAM role, ECR và artifact bucket.
- Thiết lập GitHub Actions, ECR và luồng cập nhật ECS để tự động hóa CI/CD.
- Xây dựng VPC Multi-AZ, NAT Gateway, route table và security groups.
- Triển khai ECS Fargate cho frontend/backend, Public ALB, Internal ALB, API Gateway và VPC Link.
- Cấu hình RDS MySQL Multi-AZ, Secrets Manager và RDS Proxy.
- Thiết lập CloudWatch Logs, metrics, alarms, SNS và backup.
- Tạo khung kiểm thử, rà soát bảo mật và dọn dẹp tài nguyên.

#### Nội dung

1. [Tổng quan về workshop](5.1-Workshop-overview/)
2. [Điều kiện tiên quyết](5.2-Prerequiste/)
3. [Thiết lập source, ECR và pipeline CI/CD](5.3-S3-vpc/)
4. [Triển khai runtime, data và networking](5.4-S3-onprem/)
5. [Bảo mật, tối ưu và kiểm tra mở rộng](5.5-Policy/)
6. [Dọn dẹp tài nguyên](5.6-Cleanup/)
