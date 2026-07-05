---
title: "Thiết lập S3 & Snapshot"
date: 2024-01-01
weight: 9
chapter: false
pre: " <b> 5.2.9. </b> "
---

## Mục tiêu

Tổ chức nhóm nội dung liên quan đến phần Recovery & Backup, bao gồm thiết lập RDS Automated Backup, tạo Manual Snapshot, tạo S3 Backup Bucket và Export Snapshot lên S3.

## Nội dung chính

Trong mục `5.2.9`, bạn có thể trình bày tuần tự các bước khởi tạo phần backup và recovery:

1. **RDS Automated Backup** để tự động tạo snapshot hàng ngày.
2. **Manual Snapshot** để tạo snapshot thủ công làm baseline.
3. **S3 Backup Bucket** để lưu trữ các file backup dài hạn.
4. **Export Snapshot lên S3** để lưu trữ snapshot ra S3 khi cần.

## Các trang con

1. [5.2.9.1 - RDS Automated Backup](5.2.9.1-rds-automated-backup/)
2. [5.2.9.2 - Manual Snapshot](5.2.9.2-manual-snapshot/)
3. [5.2.9.3 - S3 Backup Bucket](5.2.9.3-s3-backup-bucket/)
4. [5.2.9.4 - Export Snapshot lên S3](5.2.9.4-export-snapshot/)

## Kết quả mong đợi

Sau mục này, bạn có đầy đủ phần khung cho backup và recovery trước khi vận hành hệ thống.
