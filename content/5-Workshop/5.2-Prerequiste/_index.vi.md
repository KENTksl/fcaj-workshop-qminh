---
title : "Điều kiện tiên quyết"
date : 2024-01-01 
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---

## Mục tiêu

Trước khi bắt đầu workshop, cần chuẩn bị đầy đủ tài khoản, phân quyền, mã nguồn và các tệp cấu hình để các bước triển khai phía sau có thể thực hiện thống nhất.

## Những thành phần cần có

- **Tài khoản AWS** có quyền tạo VPC, ECS, ECR, RDS, S3, CloudWatch, SNS, IAM, API Gateway và các dịch vụ liên quan.
- **Repository GitHub** chứa source code frontend, backend, Dockerfile và các file cấu hình như `.github/workflows/*.yml`, `taskdef.json` hoặc các file environment phục vụ deploy.
- **Ứng dụng đã sẵn sàng đóng gói** để có thể build thành Docker image.
- **Thông số triển khai** như region, naming convention, port của frontend/backend, biến môi trường, thông tin database.
- **Sơ đồ kiến trúc** hoặc tài liệu thiết kế để đối chiếu trong suốt quá trình triển khai.

## Gợi ý phân quyền IAM

Bạn có thể chuẩn bị các nhóm quyền hoặc IAM roles sau:

- Role cho GitHub Actions hoặc quy trình push image lên ECR.
- Role cho GitHub Actions để build Docker image, đăng nhập ECR và cập nhật ECS service/task definition.
- Execution role và task role cho ECS services.
- Quyền cho CloudWatch, SNS, RDS monitoring và backup nếu bạn dùng các thành phần đó.

## Đầu vào nên chuẩn bị trước

- Source code frontend và backend.
- Tên tài nguyên theo quy ước thống nhất, ví dụ `globalmart-<service>-<env>`.
- Bộ file workflow cho GitHub Actions và cấu hình ECS.
- Danh sách secret hoặc biến môi trường cần đưa vào runtime.

## Kết quả mong đợi

Sau bước này, bạn đã sẵn sàng về mặt tài khoản, phân quyền và đầu vào triển khai để bắt đầu dựng hạ tầng.

## Gợi ý hình ảnh cần thêm

- Hình repository GitHub của dự án.
- Hình các IAM roles/policies chính.
- Hình cây thư mục hoặc các file cấu hình trong repo.
- Hình bảng quy ước đặt tên tài nguyên nếu bạn có chuẩn bị riêng.
