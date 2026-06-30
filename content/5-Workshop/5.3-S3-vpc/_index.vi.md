---
title : "Thiết lập mã nguồn, ECR và GitHub Actions CI/CD"
date : 2026-06-30 
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

## Mục tiêu

Phần này tập trung vào việc xây dựng luồng tự động hóa quy trình phân phối phần mềm (DevOps): đi từ mã nguồn (Source Code) trên GitHub đến việc đóng gói container image lưu trữ tập trung trên Amazon ECR, và tự động kích hoạt lệnh cập nhật trực tiếp lên môi trường chạy thực tế (ECS Fargate). Đây là bước cốt lõi giúp hệ thống GlobalMart đạt khả năng triển khai liên tục, chính xác, giảm thiểu 100% các thao tác cấu hình thủ công bằng tay.

## Dịch vụ sử dụng

- **GitHub & GitHub Actions** (Trình quản lý mã nguồn và thực thi Pipeline)
- **Amazon Elastic Container Registry - ECR** (Kho lưu trữ Docker Image tập trung)
- **AWS Identity and Access Management - IAM** (Quản lý định danh tài khoản programmatic đặc quyền)
- **Amazon ECS Fargate** (Môi trường runtime nhận lệnh deploy cuốn chiếu)

## Nội dung chương

Chương này bao gồm 2 bài thực hành chi tiết theo trình tự:

- [5.3.1. Chuẩn bị GitHub, IAM User Access Key, ECR và Thông báo](5.3.1-create-gwe/)
- [5.3.2. Cấu hình Workflow và kiểm tra luồng CI/CD](5.3.2-test-gwe/)

## Kết quả mong đợi

Sau khi hoàn thành chương này, bạn sẽ làm chủ được chu trình vận hành tự động:
- Hiểu cách bảo mật cặp khóa Access Key (Access Key ID và Secret Access Key) của AWS trên môi trường GitHub.
- Đóng gói thành công ứng dụng Frontend (Nginx/React) và Backend (Spring Boot/Node.js) thành Docker Image chuẩn chỉnh.
- Kích hoạt thành công luồng **ECS Rolling Update** tự động cập nhật container mới ngay khi có hành động `git push`.
- Nhận được Email thông báo trực quan (Xanh/Đỏ) báo cáo kết quả trạng thái xây dựng hệ thống thời gian thực.

## Gợi ý hình ảnh cần thêm

- Sơ đồ luồng dữ liệu của chu trình CI/CD từ GitHub Actions qua ECR đến ECS Cluster.
- Ảnh chụp tổng quan tab **Actions** trên GitHub hiển thị luồng chạy thành công của các bước build.