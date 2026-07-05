---
title: "Tạo Service"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.2.7.3. </b> "
---

## Mục tiêu

Tạo ECS services để triển khai frontend và backend trên cluster đã chuẩn bị.

## Các bước thực hiện

1. Truy cập **Amazon Elastic Container Service (ECS)**.
2. Chọn **Clusters** → chọn cluster `globalmart`.
3. Trong tab **Services**, bấm **Create**.

**Frontend service**

4. Ở phần **Service details**:
   - Task definition family: `globalmart-frontend-task`
   - Task definition revision: Latest
   - Service name: `globalmart-frontend-service`

   ![Service frontend setting 1](/images/5-Workshop/5.2-Prerequisite/5.2.7-ECS/service_setting1.png)
   ![Service frontend setting 2](/images/5-Workshop/5.2-Prerequisite/5.2.7-ECS/service_setting2.png)

5. Ở phần **Load balancing**:
   - Use load balancing: bật
   - Load balancer type: Application Load Balancer
   - Use an existing load balancer: `alb-internet-facing`
   - Listener: HTTP : 80
   - Target group: chọn `tg-frontend`
   - Health check path: `/`

   ![Service frontend setting 4](/images/5-Workshop/5.2-Prerequisite/5.2.7-ECS/service_setting4.png)
   ![Service frontend setting 5](/images/5-Workshop/5.2-Prerequisite/5.2.7-ECS/service_setting5.png)

**Backend service**

6. Tạo tiếp service cho backend với cấu hình:
   - Task definition family: `globalmart-backend-task`
   - Task definition revision: Latest
   - Service name: `globalmart-backend-service`

   ![Service backend setting 6](/images/5-Workshop/5.2-Prerequisite/5.2.7-ECS/service_setting6.png)

7. Ở phần **Networking**:
   - VPC: `globalmart-vpc`
   - Subnets: chọn private subnets
   - Security group: `ecs-backend-sg`
   - Public IP: Turned off

   ![Service backend setting 7](/images/5-Workshop/5.2-Prerequisite/5.2.7-ECS/service_setting7.png)

8. Ở phần **Load balancing**:
   - Use an existing load balancer: `alb-internal`
   - Listener: HTTP : 80
   - Target group: chọn `tg-backend`
   - Health check path: `/actuator/health`

   ![Service backend setting 8](/images/5-Workshop/5.2-Prerequisite/5.2.7-ECS/service_setting8.png)
   ![Service backend setting 9](/images/5-Workshop/5.2-Prerequisite/5.2.7-ECS/service_setting9.png)

## Kết quả mong đợi

- Trong cluster `globalmart`, có 2 services `globalmart-frontend-service` và `globalmart-backend-service` chạy ổn định (Active/Healthy).

  ![Service completed](/images/5-Workshop/5.2-Prerequisite/5.2.7-ECS/service_complete.png)
