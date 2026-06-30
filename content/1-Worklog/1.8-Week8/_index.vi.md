---
title: "Worklog Tuần 8"
date: 2026-05-28
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---
### Mục tiêu tuần 8:
* Hoàn thiện mô hình kiến trúc tổng thể cho project cuối kỳ để chuẩn bị cho giai đoạn triển khai thực tế.
* Phân rã rõ các thành phần chính của hệ thống gồm CI/CD, containerization, application runtime, database, backup và monitoring.
* Thống nhất được hướng triển khai project trên AWS với ECS Fargate, RDS, CloudWatch và các dịch vụ liên quan.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Phân tích yêu cầu project cuối kỳ và xác định các thành phần chính của hệ thống <br>&emsp; + Xác định mô hình triển khai frontend/backend tách riêng <br>&emsp; + Xác định hướng dùng container để đóng gói ứng dụng <br>&emsp; + Lên ý tưởng kiến trúc tổng thể trên AWS để phục vụ giai đoạn thực hành project | 08/06/2026   | 08/06/2026      | - |
| 3   | - Thiết kế khối **Build & Containerization Flow** <br>&emsp; + Xác định luồng làm việc từ GitHub đến CodePipeline <br>&emsp; + Phân vai trò của CodeBuild, CodeDeploy, ECR và Artifact Bucket <br>&emsp; + Thống nhất quy trình build image, lưu image lên ECR và chuẩn bị cho bước deploy | 09/06/2026   | 09/06/2026      | - |
| 4   | - Thiết kế khối **Deployment (Application Runtime)** <br>&emsp; + Xác định VPC, Internet Gateway, Public Subnet và Private Subnet cho hệ thống <br>&emsp; + Lên mô hình triển khai ECS Cluster trên ECS Fargate <br>&emsp; + Tách 2 service gồm Frontend và Backend <br>&emsp; + Thiết kế ALB internet-facing và ALB internal cho luồng truy cập giữa các tầng ứng dụng | 10/06/2026   | 10/06/2026      | - |
| 5   | - Thiết kế khối **Database, Recovery & Backup** <br>&emsp; + Chọn Amazon RDS for MySQL làm cơ sở dữ liệu cho hệ thống <br>&emsp; + Xác định vị trí đặt DB trong private subnet và cách kết nối từ backend <br>&emsp; + Bổ sung phương án RDS Snapshot, backup bucket và export snapshot để phục vụ backup/recovery | 11/06/2026   | 11/06/2026      | - |
| 6   | - Thiết kế khối **Monitoring & Observability** và hoàn thiện sơ đồ tổng thể <br>&emsp; + Xác định CloudWatch Logs, CloudWatch Metrics, CloudWatch Alarm cho hệ thống <br>&emsp; + Xác định SNS để gửi cảnh báo qua Email/SMS <br>&emsp; + Rà soát lại toàn bộ kết nối giữa CI/CD, ECS, RDS, backup và monitoring <br>&emsp; + Hoàn thiện mô hình kiến trúc để chuẩn bị bước thực hành project cuối kỳ | 12/06/2026   | 12/06/2026      | - |


### Kết quả đạt được tuần 8:
* Hoàn thiện được mô hình kiến trúc tổng thể cho project cuối kỳ trên AWS:
  * Xác định được khối **Build & Containerization Flow** với GitHub, CodePipeline, CodeBuild, CodeDeploy, ECR và Artifact Bucket.
  * Xác định được khối **Deployment** với VPC, Internet Gateway, Public Subnet, Private Subnet, ECS Cluster, ECS Fargate, Frontend, Backend, ALB internet-facing và ALB internal.
  * Xác định được khối **Database / Recovery & Backup** với Amazon RDS for MySQL, RDS Snapshot và Backup Bucket.
  * Xác định được khối **Monitoring & Observability** với CloudWatch Logs, CloudWatch Metrics, CloudWatch, SNS và Email/SMS notification.
* Thống nhất được luồng triển khai dự kiến cho project:
  * Source code được quản lý trên GitHub.
  * Pipeline phụ trách build, lưu artifact và đẩy container image lên Amazon ECR.
  * Ứng dụng dự kiến chạy trên ECS Fargate theo mô hình tách Frontend và Backend.
  * Backend kết nối với RDS for MySQL trong private subnet.
* Làm rõ được hướng backup và giám sát cho hệ thống trước khi bước vào giai đoạn thực hành:
  * Backup bằng RDS Snapshot và export sang backup bucket.
  * Logging, metrics và alerting được theo dõi qua CloudWatch và SNS.
* Chuẩn bị được nền tảng thiết kế để nhóm chuyển sang giai đoạn triển khai project cuối kỳ trong các tuần tiếp theo.

#### Mô hình kiến trúc đề xuất cho project cuối kỳ

![Mo hinh kien truc GlobalMart](/1-worklog/1.8-week8/GlobalMart.png)
