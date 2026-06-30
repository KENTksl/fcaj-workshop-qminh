---
title : "Chuẩn bị GitHub, IAM, ECR và Artifact Bucket"
date : 2024-01-01 
weight : 1
chapter : false
pre : " <b> 5.3.1 </b> "
---

## Mục tiêu

Chuẩn bị đầy đủ các thành phần đầu vào cho luồng triển khai, bao gồm repository, quyền truy cập vào AWS, nơi lưu Docker image và các secrets/variables dùng cho workflow.

## Các bước thực hiện

1. **Chuẩn bị GitHub repository**
- Xác nhận repo chứa source code frontend, backend và các file cấu hình phục vụ build/deploy.
- Nếu dùng monorepo, nên tách rõ thư mục ứng dụng, Dockerfile và các tệp cấu hình.

2. **Cấu hình IAM role cho quy trình đẩy image**
- Tạo IAM role cho GitHub Actions hoặc công cụ CI bạn đang dùng.
- Cấp quyền đăng nhập ECR, push image và đọc các tài nguyên cần thiết.
- Nếu dùng OIDC, cấu hình trust policy để GitHub có thể assume role an toàn.

3. **Tạo Amazon ECR repositories**
- Tạo riêng repository cho frontend và backend.
- Bật `scan on push` để quét lỗ hổng image.
- Cấu hình lifecycle policy để dọn image cũ.

4. **Chuẩn bị secrets và variables cho GitHub Actions**
- Tạo các secrets như AWS role ARN, AWS region, ECR repository name, ECS cluster name, ECS service name.
- Nếu cần, tách riêng secrets cho frontend và backend.

5. **Chuẩn bị file workflow CI/CD**
- Tạo workflow trong thư mục `.github/workflows/`.
- Khai báo các bước checkout source, login AWS, login ECR, build image, push image và cập nhật ECS.
- Chuẩn bị `taskdef.json` hoặc cơ chế render task definition phù hợp với service của bạn.

## Kết quả mong đợi

Sau bước này, bạn có:

- Repository sẵn sàng để trigger CI/CD.
- IAM role đúng mục đích.
- Hai ECR repository cho frontend và backend.
- Bộ secrets/variables cho workflow.
- Bộ file workflow cơ bản để triển khai tự động.

## Gợi ý hình ảnh cần thêm

- Hình repository GitHub.
- Hình IAM role hoặc trust policy cho GitHub Actions.
- Hình hai ECR repository.
- Hình secrets/variables hoặc file workflow GitHub Actions.
