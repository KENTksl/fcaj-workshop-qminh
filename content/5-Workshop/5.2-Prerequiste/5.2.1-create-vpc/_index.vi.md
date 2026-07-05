---
title: "Thiết lập VPC"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.2.1. </b> "
---

## Mục tiêu

Tổ chức nhóm nội dung liên quan đến lớp mạng cơ bản của hệ thống, bao gồm tạo VPC và thiết lập Security Group.

## Nội dung chính

Trong mục `5.2.1`, bạn có thể trình bày tuần tự các bước khởi tạo lớp mạng nền:

1. **Tạo VPC** để làm network boundary cho toàn bộ hệ thống.
2. **Tạo Security Group** để kiểm soát luồng truy cập giữa các thành phần như ALB, ECS và RDS.

## Các trang con

1. [5.2.1.1 - Tạo VPC](5.2.1.1-create-vpc/)
2. [5.2.1.2 - Tạo Security Group](5.2.1.2-create-security-group/)

## Kết quả mong đợi

Sau mục này, bạn có đầy đủ phần khung cho network foundation trước khi triển khai các dịch vụ compute, database và integration.
