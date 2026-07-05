---
title: "Manual Snapshot"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.2.9.2. </b> "
---

## Mục tiêu

Tạo 1 manual snapshot làm baseline trước khi bắt đầu vận hành hệ thống.

## Các bước thực hiện

1. Vào **RDS → Databases → globalmart-db**.
2. Bấm **Actions → Take snapshot**.
3. Nhập **Snapshot name**: `globalmart-db-baseline`.
4. Bấm **Take snapshot**.
5. Đợi Status chuyển thành **Available**.
![Overview](/images/5-Workshop/Workshop-img/rds_snapshot.jpg)
## Kết quả mong đợi

Có 1 manual snapshot làm điểm khôi phục đầu tiên.
