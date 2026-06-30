---
title : "Tổng quan workshop"
date : 2024-01-01 
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

## Mục tiêu của workshop

Workshop này hướng dẫn triển khai hạ tầng cho dự án **GlobalMart** theo đúng định hướng đã trình bày ở phần `Proposal`: tự động hóa quy trình CI/CD, chạy ứng dụng trên Amazon ECS Fargate, tổ chức mạng Multi-AZ, kết nối database thông qua RDS Proxy và giám sát tập trung bằng CloudWatch.

## Kiến trúc tổng thể

Hệ thống có thể chia thành 5 nhóm thành phần chính:

- **Source và CI/CD**: Developer đẩy code lên GitHub, GitHub Actions sẽ build image, đẩy image lên ECR và cập nhật phiên bản mới cho hệ thống.
- **Runtime ứng dụng**: Frontend và Backend chạy trên Amazon ECS Fargate, được cập nhật thông qua workflow triển khai từ GitHub Actions.
- **Networking và truy cập**: Public ALB tiếp nhận request từ Internet, API Gateway và VPC Link chuyển tiếp request vào lớp backend nội bộ thông qua Internal ALB.
- **Tầng dữ liệu**: Backend kết nối tới Amazon RDS MySQL Multi-AZ thông qua AWS RDS Proxy để tối ưu kết nối và tăng khả năng failover.
- **Monitoring và backup**: CloudWatch thu thập logs/metrics, SNS gửi cảnh báo và hệ thống backup hỗ trợ khôi phục dữ liệu khi có sự cố.

## Phạm vi triển khai

Trong workshop này, bạn sẽ triển khai theo các nhóm bước sau:

1. Chuẩn bị repository, IAM roles, ECR và artifact bucket.
2. Cấu hình GitHub Actions, ECR và workflow triển khai cho luồng CI/CD.
3. Tạo VPC Multi-AZ, subnet, route table, NAT Gateway và security groups.
4. Triển khai ECS Cluster, ECS Services, Public ALB, Internal ALB, API Gateway và VPC Link.
5. Tạo RDS MySQL Multi-AZ, Secrets Manager và RDS Proxy.
6. Thiết lập CloudWatch, SNS, backup và các bài kiểm thử end-to-end.

## Kết quả mong đợi

Sau khi hoàn thành workshop, bạn sẽ có:

- Bộ khung tài liệu triển khai cho toàn bộ hạ tầng dự án.
- Trình tự cấu hình rõ ràng giữa các thành phần trong kiến trúc.
- Các mục chờ để bổ sung hình ảnh thực tế từ quá trình triển khai của bạn.

## Gợi ý hình ảnh cần thêm

- Hình kiến trúc tổng thể của dự án.
- Hình mô tả luồng request từ Internet vào Frontend/Backend.
- Hình tổng quan các nhóm dịch vụ GitHub, ECR, ECS, RDS, CloudWatch và SNS.
