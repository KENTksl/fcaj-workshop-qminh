---
title: "Worklog Tuần 9"
date: 2026-05-27
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---
### Mục tiêu tuần 9:
* Xây dựng project `GlobalMart` với frontend sử dụng `React` và backend sử dụng `Java Spring Boot`.
* Hoàn thiện các chức năng cơ bản của hệ thống thương mại điện tử như đăng ký, đăng nhập, quản lý sản phẩm, danh mục và admin dashboard.
* Chuẩn bị nền tảng để triển khai `production CI/CD` cho project trên AWS.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Khởi tạo project `GlobalMart` và tổ chức cấu trúc source code <br>&emsp; + Tách 2 phần `globalmart-app` cho frontend và `globalmart-api` cho backend <br>&emsp; + Thiết lập môi trường phát triển cho `React/Vite` ở frontend và `Spring Boot` ở backend <br>&emsp; + Cấu hình chạy đồng thời frontend và backend từ thư mục root bằng `npm run dev` | 15/06/2026   | 15/06/2026      | https://github.com/KENTksl/globalmart-production-cicd |
| 3   | - Phát triển frontend với `React` và `TypeScript` <br>&emsp; + Xây dựng giao diện cơ bản cho website thương mại điện tử <br>&emsp; + Thiết kế các trang phục vụ đăng nhập, đăng ký và hiển thị sản phẩm <br>&emsp; + Kết nối frontend với backend API để chuẩn bị xử lý nghiệp vụ | 16/06/2026   | 16/06/2026      | https://github.com/KENTksl/globalmart-production-cicd |
| 4   | - Phát triển backend với `Java Spring Boot` <br>&emsp; + Xây dựng API cho đăng nhập, đăng ký người dùng <br>&emsp; + Xây dựng chức năng quản lý sản phẩm và danh mục <br>&emsp; + Kết nối backend với `MongoDB Atlas` để lưu trữ dữ liệu ứng dụng | 17/06/2026   | 17/06/2026      | https://github.com/KENTksl/globalmart-production-cicd |
| 5   | - Hoàn thiện các chức năng chính của hệ thống <br>&emsp; + Tích hợp luồng dữ liệu giữa frontend và backend <br>&emsp; + Bổ sung các chức năng `login`, `register`, `product`, `category` <br>&emsp; + Xây dựng và cập nhật `admin dashboard` để phục vụ quản trị hệ thống | 18/06/2026   | 18/06/2026      | https://github.com/KENTksl/globalmart-production-cicd |
| 6   | - Chuẩn bị nền tảng `production CI/CD` cho project <br>&emsp; + Bổ sung các file `Dockerfile`, `buildspec.yml`, `appspec.yml`, `taskdef.json` <br>&emsp; + Định hướng tích hợp `AWS CodePipeline`, `CodeBuild`, `CodeDeploy`, `ECR/EC2`, `CloudWatch` và `SNS` cho bước triển khai tiếp theo <br>&emsp; + Tổng kết tiến độ project và rà soát cấu trúc source code toàn hệ thống | 19/06/2026   | 19/06/2026      | https://github.com/KENTksl/globalmart-production-cicd |


### Kết quả đạt được tuần 9:
* Xây dựng được project `GlobalMart` theo mô hình tách frontend và backend rõ ràng:
  * Frontend sử dụng `React`, `Vite` và `TypeScript`.
  * Backend sử dụng `Java Spring Boot`.
  * Source code được tổ chức thành 2 thư mục chính là `globalmart-app` và `globalmart-api`.
* Hoàn thiện các chức năng cốt lõi của hệ thống thương mại điện tử:
  * Đăng ký tài khoản.
  * Đăng nhập hệ thống.
  * Quản lý sản phẩm.
  * Quản lý danh mục.
  * Bổ sung `admin dashboard`.
* Kết nối backend với `MongoDB Atlas` để lưu trữ và quản lý dữ liệu ứng dụng.
* Thiết lập được cách chạy project thuận tiện trong môi trường phát triển:
  * Chạy đồng thời frontend và backend bằng `npm run dev`.
  * Có thể chạy riêng backend hoặc frontend theo nhu cầu kiểm thử.
* Chuẩn bị được nền tảng để triển khai `production CI/CD` cho project:
  * Bổ sung các file cấu hình như `Dockerfile`, `buildspec.yml`, `appspec.yml`, `taskdef.json`.
  * Định hình hướng tích hợp với các dịch vụ AWS như `CodePipeline`, `CodeBuild`, `CodeDeploy`, `CloudWatch` và `SNS`.

