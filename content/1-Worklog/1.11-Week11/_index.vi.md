---
title: "Worklog Tuần 11"
date: 2026-06-29
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Mục tiêu tuần 11:

- Giải quyết bài toán kết nối `Frontend (React SPA)` với `Backend` ẩn trong private subnet
  thông qua `API Gateway` và `VPC Link`.
- Thiết lập `CI/CD pipeline` tự động hóa việc build và deploy ứng dụng lên ECS
  bằng `GitHub Actions`.
- Triển khai `Monitoring & Observability` với `CloudWatch`, `SNS` và cấu hình cảnh báo
  tự động qua email/SMS.
- Thiết lập `Recovery & Backup` với `RDS Snapshot` và export ra `S3`.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                                                                                                                                                                                                                                                                                                                                                            | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | -------------- |
| 2   | - Phân tích bài toán kết nối `React SPA` (chạy trên browser) với Backend ẩn trong private subnet <br> - Chọn giải pháp `API Gateway + VPC Link` thay cho expose trực tiếp <br> - Tạo `VPC Link` (`vpclink-globalmart`) kết nối vào `ALB internal` <br> - Tạo `HTTP API Gateway` (`globalmart-api-gateway`) với Private resource integration                                                                                                          | 29/06/2026   | 29/06/2026      |                |
| 3   | - Cấu hình `Route` trong API Gateway: `ANY /{proxy+}` → Private integration <br> - Deploy API Gateway ra stage (`$default` hoặc `prod`) <br> - Test kết nối: `curl <invoke-url>/actuator/health` trả về `{"status":"UP"}` <br> - Cập nhật `REACT_APP_API_URL` trong Frontend trỏ đúng Invoke URL <br> - Build lại Frontend image, push ECR, deploy lại ECS Service Frontend                                                                          | 30/06/2026   | 30/06/2026      |                |
| 4   | - Thiết lập `CI/CD pipeline` bằng `GitHub Actions` (thay thế CodePipeline do giới hạn quota CodeBuild) <br>&emsp; + Tạo `IAM Role` với OIDC cho GitHub Actions xác thực vào AWS <br>&emsp; + Cấu hình `GitHub Secrets`: `AWS_ROLE_ARN`, `AWS_REGION`, `ECR_REPOSITORY`, `ECS_SERVICE`, `ECS_CLUSTER`, `CONTAINER_NAME` <br>&emsp; + Viết workflow `.github/workflows/deploy-backend.yml` tự động: build image → push ECR → update ECS Service        | 01/07/2026   | 01/07/2026      |                |
| 5   | - Triển khai `S3 Buckets`: <br>&emsp; + `globalmart-backup-bucket` lưu RDS snapshot export <br> - Cấu hình `Bucket Policy` cho phép RDS export service ghi vào Backup Bucket <br> - Tạo `IAM Role` cho RDS export task <br> - Thực hiện export `RDS Snapshot` ra S3 để minh họa luồng Recovery & Backup                                                                                                                                              | 02/07/2026   | 02/07/2026      |                |
| 6   | - Triển khai `Monitoring & Observability`: <br>&emsp; + Xác nhận `CloudWatch Logs` nhận log từ ECS Frontend và Backend <br>&emsp; + Tạo `CloudWatch Alarms`: CPU > 80%, ALB 5XX errors > 5 trong 5 phút <br>&emsp; + Tạo `SNS Topic` (`globalmart-alerts`) và subscription Email/SMS <br>&emsp; + Gắn Alarm Action → SNS Topic → test nhận email cảnh báo <br> - Hoàn thiện và cập nhật sơ đồ kiến trúc tổng thể phản ánh đúng thực tế đã triển khai | 03/07/2026   | 03/07/2026      |                |

### Kết quả đạt được tuần 11:

- Giải quyết thành công bài toán kết nối `React SPA` với Backend trong private subnet:
  - Triển khai `API Gateway (HTTP API)` + `VPC Link` làm cầu nối từ Internet vào
    `ALB internal` trong VPC mà không cần expose Backend ra public.
  - Xác nhận luồng hoạt động: Browser → API Gateway → VPC Link → ALB internal
    → ECS Backend → RDS MySQL.

- Thiết lập thành công `CI/CD pipeline` bằng `GitHub Actions`:
  - Workflow tự động chạy khi push code lên branch `main`.
  - Các bước: Checkout → Configure AWS credentials (OIDC) → Login ECR →
    Build & Push image → Render Task Definition → Deploy ECS Service.
  - ECS Service tự động thực hiện `deployment` (ECS Managed)
    sau khi nhận lệnh update từ GitHub Actions.

- Triển khai thành công `Recovery & Backup`:
  - `RDS Automated Backup` (retention 7 ngày) tự động chạy hàng ngày.
  - Export RDS Snapshot ra `S3 Backup Bucket` thành công.
  - Hiểu được luồng: RDS Snapshot → Export → S3 (Snapshots/Exports).

- Triển khai thành công `Monitoring & Observability`:
  - `CloudWatch Logs` thu thập log realtime từ cả 2 ECS Service.
  - `CloudWatch Alarms` cảnh báo khi CPU vượt ngưỡng hoặc có lỗi 5XX từ ALB.
  - `SNS` gửi email/SMS tự động khi Alarm triggered.

- Hoàn thiện kiến trúc tổng thể `GlobalMart` trên AWS với đầy đủ 5 khối:
  - **Networking**: VPC, Subnet, IGW, NAT Gateway, Security Group.
  - **Deployment (Runtime)**: ECS Fargate, ALB, API Gateway, VPC Link, RDS.
  - **CI/CD**: GitHub Actions, ECR, S3 Artifact Bucket.
  - **Recovery & Backup**: RDS Snapshot, S3 Backup Bucket.
  - **Monitoring & Observability**: CloudWatch Logs, CloudWatch Metrics,
    CloudWatch Alarms, SNS, Email/SMS.
