---
title : "Thiết lập source, ECR và GitHub Actions CI/CD"
date : 2024-01-01 
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

## Mục tiêu

Phần này tập trung vào việc xây dựng luồng tự động hóa từ source code đến container image và từ container image đến môi trường runtime. Đây là bước đầu tiên để dự án có khả năng triển khai lặp lại và giảm thao tác thủ công.

## Dịch vụ sử dụng

- GitHub
- Amazon ECR
- Amazon S3
- GitHub Actions
- IAM

## Nội dung

- [Chuẩn bị GitHub, IAM, ECR và Artifact Bucket](5.3.1-create-gwe/)
- [Cấu hình GitHub Actions và kiểm tra CI/CD](5.3.2-test-gwe/)

## Kết quả mong đợi

Sau phần này, bạn có thể đẩy source code, build image, lưu image trên ECR và kích hoạt pipeline để đưa phiên bản mới vào môi trường chạy.
