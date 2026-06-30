---
title : "Dọn dẹp tài nguyên"
date : 2024-01-01
weight : 6
chapter : false
pre : " <b> 5.6. </b> "
---

## Mục tiêu

Sau khi hoàn thành workshop, cần có một bước dọn dẹp tài nguyên để tránh phát sinh chi phí không cần thiết và giữ môi trường gọn gàng.

## Thứ tự dọn dẹp gợi ý

1. Tắt hoặc vô hiệu hóa GitHub Actions workflow nếu không còn tiếp tục deploy.
2. Giảm số lượng ECS service/task hoặc xóa ECS cluster nếu đã kết thúc workshop.
3. Xóa API Gateway, VPC Link, Public ALB, Internal ALB và target groups.
4. Xóa RDS Proxy, RDS instance, secret trong Secrets Manager và các snapshot không cần thiết.
5. Xóa ECR images/repositories nếu không cần lưu trữ.
6. Làm rỗng và xóa S3 bucket dùng cho artifact hoặc backup nếu đây là tài khoản lab.
7. Xóa CloudWatch log groups, dashboard, alarms và SNS topic nếu không còn sử dụng.
8. Xóa NAT Gateway, route tables, subnet, security group và cuối cùng là VPC.

## Điểm cần lưu ý

- Không xóa nhầm tài nguyên đang được dùng cho môi trường khác.
- Kiểm tra billing sau khi dọn dẹp để đảm bảo không còn tài nguyên chi phí cao như NAT Gateway, ALB, ECS hoặc RDS.
- Nếu đây là môi trường demo dài hạn, bạn có thể chỉ tạm dừng một số thành phần thay vì xóa hoàn toàn.

## Gợi ý hình ảnh cần thêm

- Hình danh sách tài nguyên trước khi xóa.
- Hình xóa ECS/ALB/RDS.
- Hình billing hoặc resource list sau khi dọn dẹp.
