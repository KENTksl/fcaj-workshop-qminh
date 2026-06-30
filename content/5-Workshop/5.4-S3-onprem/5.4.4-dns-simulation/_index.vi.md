---
title : "Thiết lập CloudWatch, Backup và kiểm thử hệ thống"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.4.4 </b> "
---

## Mục tiêu

Bổ sung lớp quan sát vận hành, cảnh báo và dự phòng để hệ thống không chỉ chạy được mà còn dễ theo dõi, dễ phát hiện sự cố và dễ khôi phục khi cần.

## Các bước thực hiện

1. **Cấu hình CloudWatch Logs**
- Gửi log từ frontend và backend containers lên CloudWatch Logs.
- Tách riêng log group theo từng thành phần để dễ truy vết.

2. **Thu thập metrics và tạo dashboard**
- Theo dõi ALB, ECS, RDS, CPU, memory, request count, latency và các chỉ số quan trọng.
- Tạo một CloudWatch Dashboard tổng hợp để theo dõi toàn hệ thống.

3. **Tạo alarms và SNS notifications**
- Cấu hình CloudWatch Alarms cho CPU cao, lỗi application, latency tăng, RDS bất thường hoặc health check thất bại.
- Gắn SNS topic để gửi cảnh báo qua email hoặc kênh thông báo phù hợp.

4. **Cấu hình backup và recovery**
- Bật Automated Backup cho RDS.
- Chuẩn bị S3 backup bucket nếu bạn có cơ chế export snapshot hoặc lưu file backup.
- Xác định quy trình khôi phục dữ liệu khi gặp sự cố.

5. **Kiểm thử end-to-end**
- Thử truy cập frontend từ Internet.
- Thử gọi API backend qua API Gateway.
- Thử thao tác đọc/ghi database thông qua backend.
- Nếu có thể, mô tả thêm bài kiểm thử failover hoặc DR drill.

## Kết quả mong đợi

Sau bước này, hệ thống đã có khả năng giám sát, cảnh báo và dự phòng cơ bản để phục vụ vận hành ổn định hơn trong môi trường production.

## Gợi ý hình ảnh cần thêm

- Hình CloudWatch Logs.
- Hình dashboard tổng hợp.
- Hình CloudWatch Alarms và SNS topic.
- Hình backup, snapshot hoặc kết quả kiểm thử end-to-end.
