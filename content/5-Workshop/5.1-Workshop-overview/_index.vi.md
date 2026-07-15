---
title : "Tổng quan workshop"
date : 2026-06-30
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

## Mục tiêu của workshop

Workshop này hướng dẫn triển khai hạ tầng cho dự án **GlobalMart** theo đúng định hướng đã trình bày ở phần `Proposal`: tự động hóa chuỗi quy trình CI/CD qua **GitHub Actions** ứng dụng cơ chế xác thực bảo mật **IAM OIDC**, vận hành các ứng dụng trên nền tảng **Amazon ECS Fargate**, tổ chức phân lớp mạng mạng VPC cô lập (Public/Private Subnets), kiểm soát và định tuyến lưu lượng API tập trung qua **Amazon API Gateway** kết hợp **VPC Link**, tự động hóa gửi email báo cáo kết quả hệ thống thời gian thực và xây dựng bộ giám sát, cảnh báo đa tầng với **CloudWatch Dashboards & Alarms**.

## Kiến trúc tổng thể

Hệ thống có thể chia thành 5 nhóm thành phần chính:

- **Source và CI/CD**: Developer đẩy code lên GitHub. GitHub Actions sẽ tự động kích hoạt workflow, xác thực bảo mật qua IAM OIDC Role, đóng gói Docker Image, đẩy lên Amazon ECR, tự động hóa luồng ECS Rolling Update và gửi mail thông báo trạng thái kết quả thời gian thực.
- **Runtime ứng dụng**: Frontend và Backend vận hành dưới dạng các container không máy chủ (Serverless) độc lập trên nền tảng AWS Fargate, nằm ẩn hoàn toàn trong phân vùng Private Subnet an toàn.
- **Networking và truy cập**: Public ALB chịu trách nhiệm tiếp nhận và điều hướng traffic trực tiếp từ Internet vào container Frontend. Toàn bộ các yêu cầu gọi API công khai sẽ được kiểm soát tập trung tại Amazon API Gateway, sau đó chuyển tiếp an toàn qua hệ thống VPC Link và Internal ALB để đi vào container Backend.
- **Tầng dữ liệu**: Container Backend kết nối trực tiếp qua mạng nội bộ bảo mật tới cơ sở dữ liệu Amazon RDS Single AZ được đặt sâu trong vùng Private Subnet B, chặn tuyệt đối mọi hành vi truy cập trái phép từ Internet.
- **Monitoring và backup**: CloudWatch tập trung thu thập toàn bộ nhật ký container (Logs), chỉ số mạng và hiệu năng database để hiển thị trực quan lên Dashboard hệ thống, kết hợp với Amazon SNS để chủ động bắn email/SMS cảnh báo khẩn cấp khi hệ thống quá tải.

## Phạm vi triển khai

Trong workshop này, bạn sẽ triển khai theo các nhóm bước sau:

1. Chuẩn bị tài nguyên trên GitHub, cấu hình liên kết định danh AWS IAM OIDC Identity Providers và thiết lập các IAM Execution Roles đặc quyền.
2. Khởi tạo kho lưu trữ Amazon ECR, xây dựng và tối ưu hóa tệp cấu hình workflow triển khai (`.yml`) tự động hóa các bước build, tag và push image.
3. Thiết lập mạng lõi VPC, phân định rõ ràng hệ thống Public Subnet, Private Subnets, Route Tables và cấu hình kết nối Internet qua NAT Gateway.
4. Xây dựng ma trận bảo mật mạng nội bộ bằng cách cấu hình 3 nhóm Security Groups chuyên biệt để kiểm soát luồng traffic đi vào giữa các tầng dịch vụ.
5. Khởi tạo cụm điều phối container Amazon ECS Cluster, cấu hình Task Definitions, thiết lập ECS Services chạy song song với cặp bộ cân bằng tải Public ALB và Internal ALB.
6. Tích hợp cổng kiểm soát API Gateway và kết nối VPC Link để hoàn thiện luồng định tuyến API nội bộ cho Backend.
7. Triển khai cơ sở dữ liệu Amazon RDS Single AZ, thiết lập các biến môi trường kết nối ứng dụng và cấu hình lịch trình sao lưu tự động (Backup).
8. Khởi tạo hệ thống CloudWatch Log Groups, thiết kế giao diện đồ thị CloudWatch Dashboard tập trung, cấu hình bộ cảnh báo Alarms và đăng ký nhận mail qua Amazon SNS.

## Kết quả mong đợi

Sau khi hoàn thành workshop, bạn sẽ có:

- Bộ khung tài liệu triển khai chi tiết cho toàn bộ hạ tầng thực tế của dự án.
- Trình tự các bước cấu hình, tích hợp và ràng buộc bảo mật rõ ràng giữa các thành phần trong kiến trúc.
- Các mục chờ sẵn để bổ sung hình ảnh thực tế chụp lại từ quá trình thao tác trên AWS Console của bạn.

## Sơ đồ kiến trúc tổng thể mới của dự án

![Overview](/images/5-Workshop/5.1-Workshop-overview/globalmart.png)