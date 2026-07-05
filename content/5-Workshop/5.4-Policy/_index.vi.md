---
title: "Bảo mật, tối ưu và kiểm tra mở rộng"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

## Mục tiêu

Trang này dùng để tổng hợp các nội dung nâng cao giúp workshop tiến gần hơn tới một môi trường production-ready, bao gồm rà soát bảo mật, tối ưu vận hành và kiểm tra thêm một số tình huống quan trọng.

## Nội dung chính

Mục `5.4` được tổ chức theo các nhóm nội dung nâng cao:

1. **Rà soát least privilege** - Kiểm tra lại IAM roles, task roles và security groups, đảm bảo mỗi thành phần chỉ có quyền đúng mục đích sử dụng.
2. **Kích hoạt và theo dõi scan bảo mật** - Xác minh ECR scan on push hoạt động cho frontend và backend image, mô tả cách xử lý image có cảnh báo lỗ hổng.
3. **Bổ sung logs và truy vết** - Cân nhắc bổ sung VPC Flow Logs, ALB access logs hoặc API Gateway logs, đảm bảo có đủ dữ liệu cho việc phân tích nguyên nhân sự cố.
4. **Tối ưu chi phí và hiệu năng** - Xem lại kích thước ECS task, cấu hình RDS, chu kỳ backup và lifecycle policy của ECR/S3, đánh giá có nên bật autoscaling hoặc điều chỉnh ngưỡng alarm.
5. **Kiểm tra mở rộng** - Thử rollout một phiên bản mới qua GitHub Actions, thử service restart hoặc task replacement trên ECS, mô tả một tình huống failover hoặc khôi phục từ backup.

## Kết quả mong đợi

Sau mục này, bạn đã bổ sung phần đánh giá sau triển khai, thể hiện dự án không chỉ dừng ở việc chạy được mà còn hướng tới vận hành an toàn, tiết kiệm và ổn định.

## Gợi ý hình ảnh cần thêm

- Hình kết quả ECR scan.
- Hình rà soát IAM role hoặc security group matrix.
- Hình VPC Flow Logs, ALB logs hoặc API Gateway logs.
- Hình dashboard hiệu năng hoặc biểu đồ chi phí nếu bạn có.
