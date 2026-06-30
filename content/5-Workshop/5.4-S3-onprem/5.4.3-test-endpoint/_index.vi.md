---
title : "Cấu hình RDS Multi-AZ, Secrets Manager và RDS Proxy"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.4.3 </b> "
---

## Mục tiêu

Thiết lập tầng dữ liệu có tính sẵn sàng cao và kết nối an toàn để backend có thể đọc ghi dữ liệu mà không truy cập trực tiếp vào database endpoint.

## Các bước thực hiện

1. **Tạo DB Subnet Group**
- Chọn các private subnets dành riêng cho data layer.
- Đảm bảo subnet group trải đều trên 2 Availability Zones.

2. **Tạo Amazon RDS MySQL Multi-AZ**
- Chọn engine, instance class, storage và chế độ Multi-AZ phù hợp.
- Cấu hình backup retention, maintenance window và security group.

3. **Lưu thông tin bí mật trong Secrets Manager**
- Lưu username, password và các biến kết nối database.
- Hạn chế việc hard-code thông tin đăng nhập trong ứng dụng.

4. **Tạo AWS RDS Proxy**
- Gắn proxy với database vừa tạo.
- Cấu hình authentication bằng secret trong Secrets Manager.
- Chỉ định security group và subnet cho proxy.

5. **Tích hợp backend với RDS Proxy**
- Cập nhật connection string của backend trỏ đến proxy endpoint.
- Kiểm tra các thao tác đọc/ghi dữ liệu và xác nhận ứng dụng vẫn hoạt động đúng.

## Kết quả mong đợi

Sau bước này, backend kết nối tới tầng dữ liệu thông qua RDS Proxy, giảm rủi ro cạn kiệt kết nối và tăng khả năng failover minh bạch hơn.

## Gợi ý hình ảnh cần thêm

- Hình DB Subnet Group.
- Hình RDS MySQL Multi-AZ.
- Hình secret trong Secrets Manager.
- Hình RDS Proxy endpoint và phần kiểm tra kết nối từ backend.



