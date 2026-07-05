---
title: "Tạo APIs"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.2.6.1. </b> "
---

## Mục tiêu

Tạo các API resources, routes và integration cần thiết trên API Gateway.

## Các bước thực hiện

1. Vào **API Gateway Dashboard** trên AWS Management Console.
2. Chọn **APIs** → **Create API**.
3. Chọn **HTTP API** → **Build**.

   ![API Setting 1](/images/5-Workshop/5.2-Prerequisite/5.2.6-APIGateway/api_setting1.png)

4. Cấu hình API:
   - API name: `globalmart-api`
   - IP address type: IPv4
   - Chưa cần tạo Integration ở bước này (để tạo sau)

   ![API Setting 2](/images/5-Workshop/5.2-Prerequisite/5.2.6-APIGateway/api_setting2.png)

5. Tạo route:
   - Vào **Routes** → **Create**
   - Route and method: `ANY /{proxy+}`

   ![API Setting 3](/images/5-Workshop/5.2-Prerequisite/5.2.6-APIGateway/api_setting3.png)

6. Tạo integration (đi qua VPC Link tới internal ALB):
   - Vào **Integrations** → **Manage integrations** → **Create**
   - Integration type: **Private resource**
   - Selection method: **Select manually**
   - Target service: **ALB/NLB**
   - Load balancer: `alb-internal`
   - Listener: HTTP : 80
   - VPC link: `vpclink-globalmart` (tạo ở 5.2.6.2)

   ![API Setting 4](/images/5-Workshop/5.2-Prerequisite/5.2.6-APIGateway/api_setting4.png)

7. Gán integration cho route:
   - Quay lại **Routes** → chọn route `ANY /{proxy+}`
   - Chọn integration vừa tạo và bấm **Attach integration**

   ![API Setting 5](/images/5-Workshop/5.2-Prerequisite/5.2.6-APIGateway/api_setting5.png)

## Kết quả mong đợi

- HTTP API `globalmart-api` có route `ANY /{proxy+}` và forward vào `alb-internal` thông qua VPC Link.
