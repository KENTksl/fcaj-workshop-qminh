---
title: "Tạo Task Definitions"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.2.7.1. </b> "
---

## Mục tiêu

Tạo task definitions cho frontend và backend để ECS có thể chạy container đúng cấu hình.

## Các bước thực hiện

1. Truy cập **Amazon Elastic Container Service (ECS)**.
2. Chọn **Task definitions** → **Create new task definition**.

3. Tạo **Frontend task definition**:
   - Task definition family: `globalmart-frontend-task`
   - Launch type: **AWS Fargate**
   - OS/Architecture: Linux/x86_64
   - Task size: CPU **1 vCPU**, Memory **3 GB**

   ![Task definitions 1](/images/5-Workshop/5.2-Prerequisite/5.2.7-ECS/task_setting1.png)

4. Cấu hình container `frontend`:
   - Container name: `frontend`
   - Image URI: chọn từ ECR repository `globalmart-frontend`
   - Port mappings: container port **80** (TCP), app protocol **HTTP**

   ![Task definitions 2](/images/5-Workshop/5.2-Prerequisite/5.2.7-ECS/task_setting2.png)

   Chọn image từ ECR (nếu repo chưa có image thì danh sách sẽ hiển thị “No images”).

   ![Task definitions 3](/images/5-Workshop/5.2-Prerequisite/5.2.7-ECS/task_setting3.png)

5. Tạo **Backend task definition** với cấu hình tương tự:
   - Task definition family: `globalmart-backend-task`
   - Container name: `backend`
   - Image URI: chọn từ ECR repository `globalmart-backend`
   - Container port: **8080**
   - Environment variables (không ghi password vào tài liệu):
     - `SPRING_DATASOURCE_URL`
     - `SPRING_DATASOURCE_USERNAME`
     - `SPRING_DATASOURCE_PASSWORD`

## Kết quả mong đợi

- Bạn có 2 task definitions `globalmart-frontend-task` và `globalmart-backend-task` (Active).

  ![Task complete](/images/5-Workshop/5.2-Prerequisite/5.2.7-ECS/task_complete.png)
