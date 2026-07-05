---
title: "Thiết lập ECR"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.2.3. </b> "
---

## Mục tiêu

Chuẩn bị Amazon ECR để lưu trữ Docker image cho frontend và backend.

## Các bước thực hiện

1. Truy cập **ECR Dashboard** trên AWS Management Console.
2. Chọn **Private registry** → **Repositories** → **Create repository**.

3. Tạo repository `globalmart-frontend`:
   - Repository name: `globalmart-frontend`
   - Image tag mutability: **Mutable**
   - Encryption: **AES-256**

   ![ECR Setting 1](/images/5-Workshop/5.2-Prerequisite/5.2.3-ECR/ecr_setting1.png)

4. Tạo repository `globalmart-backend` với cấu hình tương tự:

   ![ECR Setting 2](/images/5-Workshop/5.2-Prerequisite/5.2.3-ECR/ecr_setting2.png)

## Kết quả mong đợi

- Bạn có 2 repositories `globalmart-frontend` và `globalmart-backend` trong ECR để dùng ở bước tạo ECS task definitions.

  ![ECR Completed](/images/5-Workshop/5.2-Prerequisite/5.2.3-ECR/ecr_complete.png)
