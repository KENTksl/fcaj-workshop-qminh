---
title: "Tạo VPC Link"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.2.6.2. </b> "
---

## Mục tiêu

Tạo VPC Link để API Gateway có thể kết nối tới các tài nguyên nội bộ trong VPC.

## Các bước thực hiện

1. Vào **API Gateway Dashboard** trên AWS Management Console.
2. Chọn **VPC Links**
3. Chọn **Create** và cấu hình:
   - VPC link version: **VPC link V2**
   - Name: `vpclink-globalmart`
   - VPC: `globalmart-vpc`
   - Subnets: chọn **private subnets** (theo VPC đã tạo)
   - Security groups: chọn `alb-internal-sg`

   ![VPC Link Setting](/images/5-Workshop/5.2-Prerequisite/5.2.6-APIGateway/vpclink_setting1.png)

4. Bấm **Create** và chờ trạng thái available.

## Kết quả mong đợi

- VPC Link `vpclink-globalmart` được tạo thành công để dùng cho bước tạo Integration của HTTP API.

  ![VPC Link Complete](/images/5-Workshop/5.2-Prerequisite/5.2.6-APIGateway/vpclink_complete.png)
