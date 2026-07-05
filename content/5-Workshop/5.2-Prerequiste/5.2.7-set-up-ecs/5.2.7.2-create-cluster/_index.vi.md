---
title: "Tạo Cluster"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.2.7.2. </b> "
---

## Mục tiêu

Tạo ECS cluster để chạy các services của hệ thống GlobalMart.

## Các bước thực hiện

1. Truy cập **Amazon Elastic Container Service (ECS)**.
2. Chọn **Clusters** → **Create cluster**.
3. Cấu hình:
   - Cluster name: `globalmart`
   - Infrastructure: **Fargate only**

   ![Cluster setting](/images/5-Workshop/5.2-Prerequisite/5.2.7-ECS/cluster_setting1.png)

4. Bấm **Create** để tạo cluster.

## Kết quả mong đợi

- ECS cluster `globalmart` được tạo thành công để triển khai services ở bước tiếp theo.
