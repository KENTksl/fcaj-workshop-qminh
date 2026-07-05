---
title: "Chuẩn bị mã nguồn, IAM OIDC và GitHub Actions CI/CD"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

## Mục tiêu

Phần này tập trung xây dựng quy trình tự động hóa từ mã nguồn đến Docker Image và từ Docker Image đến môi trường triển khai. Đây là bước đầu tiên giúp dự án có khả năng triển khai lặp lại một cách nhất quán, đồng thời giảm thiểu các thao tác thủ công trong quá trình phát hành phiên bản mới.

## Nội dung chính

Mục `5.3` được chia thành các bước quan trọng trong quy trình CI/CD như sau:

1. **Chuẩn bị IAM OIDC** để GitHub Actions có thể xác thực với AWS một cách an toàn, không cần sử dụng Access Key.
2. **Cấu hình Workflow GitHub Actions** nhằm tự động build Docker Image, đẩy Image lên Amazon ECR và triển khai ứng dụng lên Amazon ECS.

## Các bài thực hành

1. [5.3.1 - Chuẩn bị IAM OIDC](5.3.1-set-up-iam-oidc/)
2. [5.3.2 - Cấu hình Workflow GitHub Actions CI/CD](5.3.2-set-up-github-action/)

## Kết quả mong đợi

Sau khi hoàn thành phần này, bạn sẽ có một quy trình CI/CD hoàn chỉnh, cho phép:

- Đẩy mã nguồn lên GitHub.
- Tự động kích hoạt GitHub Actions khi có thay đổi trên nhánh chính.
- Build Docker Image cho Frontend và Backend.
- Đẩy Docker Image lên Amazon ECR.
- Tự động cập nhật phiên bản mới lên Amazon ECS.
- Giảm thiểu thao tác triển khai thủ công, đồng thời nâng cao tính nhất quán và bảo mật của quy trình triển khai.