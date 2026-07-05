---
title: "Dọn dẹp tài nguyên"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

## Mục tiêu

Sau khi hoàn thành workshop, cần xóa các tài nguyên AWS đã tạo để tránh phát sinh chi phí không cần thiết và đảm bảo môi trường AWS luôn gọn gàng.

---

## Các bước thực hiện

Nên xóa tài nguyên theo thứ tự dưới đây để tránh lỗi do phụ thuộc giữa các dịch vụ.

### 1. Dừng GitHub Actions (tùy chọn)

Nếu không còn tiếp tục triển khai ứng dụng, bạn có thể:

- Vô hiệu hóa (Disable) GitHub Actions Workflow.
- Hoặc xóa file workflow trong thư mục `.github/workflows/`.

---

### 2. Xóa dịch vụ Amazon ECS

Truy cập **Amazon ECS**:

- Xóa ECS Services:
  - `globalmart-frontend-service`
  - `globalmart-backend-service`
- Chờ tất cả Tasks dừng hoàn toàn.
- Xóa các Task Definition không còn sử dụng (tùy chọn).
- Xóa ECS Cluster `globalmart-cluster`.

---

### 3. Xóa Application Load Balancer

Truy cập **EC2 → Load Balancers**:

- Xóa Public Application Load Balancer.
- Xóa Internal Application Load Balancer (nếu có).

Sau đó tiếp tục xóa:

- Target Groups.
- Listeners.

---

### 4. Xóa API Gateway

Truy cập **Amazon API Gateway**:

- Chọn API của dự án.
- Delete API.

Xóa VPC Link sau khi API đã được xóa.

---

### 5. Xóa Amazon RDS

Truy cập **Amazon RDS**:

- Xóa RDS Proxy.
- Xóa Database Instance.
- Không tạo Final Snapshot nếu không cần lưu dữ liệu.
- Xóa các Snapshot thủ công còn lại (nếu có).

---

### 6. Xóa Secrets Manager

Truy cập **AWS Secrets Manager**:

- Xóa Secret chứa thông tin kết nối Database.

---

### 7. Xóa Amazon ECR

Truy cập **Amazon ECR**:

- Xóa toàn bộ Docker Images.
- Xóa Repository:
  - `globalmart-frontend`
  - `globalmart-backend`

---

### 8. Xóa Amazon S3

Nếu có sử dụng S3:

- Làm rỗng (Empty) Bucket.
- Xóa Bucket.

---

### 9. Xóa CloudWatch và SNS

Truy cập **CloudWatch**:

- Xóa Log Groups.
- Xóa Dashboard.
- Xóa Alarms.

Truy cập **Amazon SNS**:

- Xóa Topic `globalmart-alerts`.

---

### 10. Xóa hạ tầng mạng

Truy cập **Amazon VPC** và lần lượt xóa:

- NAT Gateway
- Route Tables (không phải Main Route Table)
- Internet Gateway
- Subnets
- Security Groups (ngoài Default)
- Cuối cùng xóa VPC

---

## Lưu ý

- Luôn xóa tài nguyên theo đúng thứ tự để tránh lỗi phụ thuộc.
- Kiểm tra lại bảng điều khiển Billing sau vài phút để chắc chắn không còn dịch vụ đang chạy.
- Nếu muốn tiếp tục sử dụng dự án trong tương lai, bạn chỉ nên dừng các dịch vụ thay vì xóa hoàn toàn.

---

## Kết quả mong đợi

Sau khi hoàn thành bài thực hành này:

| Thành phần | Trạng thái |
|---|---|
| GitHub Actions | Đã dừng hoặc vô hiệu hóa |
| Amazon ECS | Đã xóa Cluster và Services |
| Application Load Balancer | Đã xóa |
| Amazon API Gateway | Đã xóa |
| Amazon RDS | Đã xóa |
| AWS Secrets Manager | Đã xóa Secrets |
| Amazon ECR | Đã xóa Images và Repository |
| Amazon S3 | Đã xóa Bucket (nếu có) |
| CloudWatch | Đã xóa Logs, Dashboard và Alarms |
| Amazon SNS | Đã xóa Topic |
| Amazon VPC | Đã xóa toàn bộ tài nguyên mạng |