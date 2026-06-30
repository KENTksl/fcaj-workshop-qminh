---
title: "Worklog Tuần 4"
date: 2026-05-11
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---
### Mục tiêu tuần 4:

* Hoàn thành Module 4 và nắm toàn bộ kiến thức về dịch vụ lưu trữ trên AWS (S3, Snow Family, Storage Gateway, Disaster Recovery).
* Hiểu chi tiết S3 (Object Storage, bucket/object, REST API, Access Point, Static Website Hosting, CORS, Versioning, VPC Endpoint, Storage Classes, Object Lifecycle Management).
* Nắm được Snow Family, Storage Gateway (File/Volume/Tape Gateway) và Disaster Recovery (RTO/RPO, các chiến lược Backup & Restore, Pilot Light, Low Capacity, Full Capacity).

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Học lý thuyết Amazon S3: Object Storage, bucket/object, REST API (GET, PUT), cấu trúc lưu trữ và cơ chế hoạt động <br>&emsp; + Tìm hiểu tính năng S3: Access Point, Static Website Hosting, CORS, Versioning, VPC Endpoint <br>&emsp; + Nghiên cứu lớp lưu trữ S3 (Standard, Standard-IA, Intelligent-Tiering, One Zone-IA, Glacier) và Object Lifecycle Management để tối ưu chi phí                                                                                             | 11/05/2026   | 11/05/2026      | https://youtu.be/_yunukwcAwc?si=poUK5WYhHGfiKbn5 <br> https://youtu.be/mPBjB6Ltl_Q?si=WCTj5ubOenpXb65 |
| 3   | - Học Snow Family (Snowball, Snowball Edge, Snowmobile) và ứng dụng chuyển đổi dữ liệu dung lượng lớn <br>&emsp; + Tìm hiểu AWS Storage Gateway: File Gateway, Volume Gateway, Tape Gateway trong mô hình Hybrid Storage <br>&emsp; + Nghiên cứu Disaster Recovery: khái niệm RTO, RPO và các chiến lược Backup & Restore, Pilot Light, Low Capacity, Full Capacity                                                                                         | 12/05/2026   | 12/05/2026      | https://youtu.be/YXn8Q_Hpsu4?si=8u2aAr0Dn8BQgewR |
| 4   | - **Lab 13:** Triển khai AWS Backup cho hệ thống (backup plan cho EBS/RDS/DynamoDB/EFS, khôi phục dữ liệu, tự động hóa + SNS thông báo) <br>- **Lab 14:** VM Import/Export (import VM vào EC2, sao lưu VM vào EC2)                                                                                         | 13/05/2026   | 13/05/2026      | https://000013.awsstudygroup.com <br> https://000014.awsstudygroup.com |
| 5   | - **Lab 24:** Triển khai File Storage Gateway (tạo Storage Gateway, File Shares, kết nối On-premise) <br>- **Lab 25 (Lab bị outdate nên không thực hiện được):** Đọc qua FSx (tìm hiểu kiến trúc FSx, file servers, storage, VPC, networking, data replication)                                                                                         | 14/05/2026   | 14/05/2026      | https://000024.awsstudygroup.com <br> https://000025.awsstudygroup.com|
| 6   | - **Lab 57 (Chưa làm được Cloudfront vì bị giới hạn quyền):** Khởi đầu với Amazon S3 (host website tĩnh, quản lý object, cấu hình truy cập) <br>- Tổng kết toàn bộ nội dung Module 4                                                                                         | 15/05/2026   | 15/05/2026      | https://000057.awsstudygroup.com |


### Kết quả đạt được tuần 4:

* Hoàn thành Module 4 và nắm toàn bộ kiến thức về dịch vụ lưu trữ trên AWS:
  * Amazon S3: Object Storage, bucket/object, REST API, Access Point, Static Website Hosting, CORS, Versioning, VPC Endpoint, các lớp lưu trữ và Object Lifecycle Management.
  * Snow Family (Snowball, Snowball Edge, Snowmobile) và ứng dụng chuyển dữ liệu quy mô lớn.
  * AWS Storage Gateway: File Gateway, Volume Gateway, Tape Gateway (Hybrid Storage).
  * Disaster Recovery trên AWS: khái niệm RTO/RPO và các chiến lược (Backup & Restore, Pilot Light, Low Capacity, Full Capacity).
* Hiểu chi tiết và phân biệt được các lớp lưu trữ S3, cách tối ưu chi phí với Lifecycle Management.
* Nắm được các tính năng kiểm soát truy cập và triển khai S3 (Access Point, VPC Endpoint, Static Website, CORS, Versioning).
* Thực hành toàn bộ các lab theo lộ trình Module 4:
  * **Lab13**: Triển khai AWS Backup cho hệ thống (backup plan cho EBS/RDS/DynamoDB/EFS, khôi phục dữ liệu, tự động hóa + SNS thông báo).
  * **Lab14**: VM Import/Export (import VM vào EC2, sao lưu VM vào EC2).
  * **Lab24**: Triển khai File Storage Gateway (tạo Storage Gateway, File Shares, kết nối On-premise).
  * **Lab57**: Khởi đầu với Amazon S3 (host website tĩnh, quản lý object, cấu hình truy cập).


