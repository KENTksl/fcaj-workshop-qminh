---
title : "Thiết lập VPC, IAM và lớp bảo mật"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.4.1 </b> "
---

## Mục tiêu

Tạo lớp hạ tầng nền cho toàn bộ hệ thống gồm VPC Multi-AZ, subnet, route table, NAT Gateway, security groups và IAM roles cho các thành phần runtime.

## Các bước thực hiện

1. **Tạo VPC và subnet**
- Tạo 1 VPC chính cho dự án.
- Tạo 2 public subnets và 2 private subnets trải đều trên 2 Availability Zones.
- Đánh dấu rõ subnet nào dùng cho ALB, ECS services và data layer.

2. **Cấu hình Internet Gateway và NAT Gateway**
- Gắn Internet Gateway cho VPC để phục vụ truy cập public.
- Tạo 2 NAT Gateways, mỗi AZ một NAT để tránh SPOF.
- Tách route table cho public và private subnet.

3. **Thiết kế security groups**
- Tạo security group cho Public ALB, Internal ALB, ECS Frontend, ECS Backend, RDS Proxy và RDS.
- Chỉ mở các kết nối cần thiết theo nguyên tắc least privilege.

4. **Tạo IAM roles**
- Tạo execution role và task role cho ECS.
- Tạo role cho GitHub Actions, ECS và các thành phần monitoring nếu cần.

## Kết quả mong đợi

Sau bước này, bạn có một bộ khung mạng và bảo mật sẵn sàng để gắn các thành phần ứng dụng và dữ liệu vào đúng vị trí.

## Gợi ý hình ảnh cần thêm

- Hình VPC và 4 subnets.
- Hình route tables và 2 NAT Gateways.
- Hình danh sách security groups.
- Hình các IAM roles chính.


