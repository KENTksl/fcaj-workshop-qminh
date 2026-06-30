---
title : "Cấu hình GitHub Actions và kiểm tra CI/CD"
date : 2024-01-01 
weight : 2
chapter : false
pre : " <b> 5.3.2 </b> "
---

## Mục tiêu

Hoàn thiện luồng tự động build và deploy để mỗi lần cập nhật code có thể được đưa vào hệ thống một cách nhất quán.

## Các bước thực hiện

1. **Tạo workflow GitHub Actions**
- Tạo file workflow trong `.github/workflows/`.
- Khai báo trigger theo `push` hoặc `workflow_dispatch`.
- Cấu hình các bước checkout source, cấu hình AWS credentials và login ECR.

2. **Build và push Docker image**
- Build image frontend và backend theo Dockerfile tương ứng.
- Tag image theo commit SHA hoặc version của bạn.
- Push image mới lên ECR.

3. **Cập nhật ECS service**
- Render task definition mới với image vừa build.
- Dùng GitHub Actions để đăng ký revision mới và cập nhật ECS service.
- Nếu cần, tách workflow riêng cho frontend và backend.

4. **Thử trigger workflow**
- Commit một thay đổi nhỏ vào repository.
- Theo dõi quá trình build image, push image và cập nhật ECS service.
- Xác nhận image mới xuất hiện trong ECR và task mới chạy thành công trên ECS.

## Điểm cần xác minh

- Workflow chạy đúng branch và đúng trigger.
- Image được build đúng tag và push thành công lên ECR.
- ECS service nhận revision mới và rollout thành công.
- Frontend/backend sau deploy vẫn hoạt động đúng health check.

## Kết quả mong đợi

Sau bước này, hệ thống đã có luồng CI/CD tự động để build và triển khai phiên bản mới, giảm thao tác tay trong vận hành.

## Gợi ý hình ảnh cần thêm

- Hình workflow GitHub Actions chạy thành công.
- Hình bước login ECR, build và push image.
- Hình ECS service rollout thành công.
- Hình image mới trong ECR sau khi push.
