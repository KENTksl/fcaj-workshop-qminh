---
title: "Thiết lập dịch vụ AWS"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

## Mục tiêu

Tạo bộ khung triển khai cho các dịch vụ AWS chính trong kiến trúc GlobalMart để bạn có thể tiếp tục điền chi tiết từng bước thực hành ở các mục con.

## Nội dung chính

Mục `5.2` được tổ chức theo từng nhóm dịch vụ AWS quan trọng trong hệ thống:

1. **VPC** cho lớp mạng và nhóm bảo mật.
2. **RDS** cho subnet group và database.
3. **ECR** cho kho chứa Docker image.
4. **Load Balancers** cho điều phối lưu lượng truy cập.
5. **Target Group** cho liên kết giữa load balancer và services.
6. **API Gateway** cho public API và kết nối VPC Link.
7. **ECS** cho task definitions, cluster và service.
8. **CloudWatch & SNS** cho giám sát và cảnh báo.
9. **S3 & Snapshot** cho backup và recovery.

## Các trang con

1. [5.2.1 - Thiết lập VPC](5.2.1-create-vpc/)
2. [5.2.2 - Thiết lập RDS](5.2.2-set-up-rds/)
3. [5.2.3 - Thiết lập ECR](5.2.3-set-up-ecr/)
4. [5.2.4 - Thiết lập Load Balancers](5.2.4-set-up-load-balancers/)
5. [5.2.5 - Thiết lập Target Group](5.2.5-set-up-target-group/)
6. [5.2.6 - Thiết lập API Gateway](5.2.6-set-up-api-gateway/)
7. [5.2.7 - Thiết lập ECS](5.2.7-set-up-ecs/)
8. [5.2.8 - Thiết lập CloudWatch & SNS](5.2.8-set-up-cloudwatch-sns/)
9. [5.2.9 - Thiết lập S3 & Snapshot](5.2.9-set-up-s3-snapshot/)

## Kết quả mong đợi

Sau mục này, bạn có sẵn khung nội dung theo dịch vụ để tiếp tục hoàn thiện toàn bộ workshop triển khai GlobalMart trên AWS.
