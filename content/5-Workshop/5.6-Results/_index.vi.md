---
title: "Kết quả thực tế"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

## Mục tiêu

Tổng hợp các kết quả thực tế sau khi chạy thử dự án, bao gồm quy trình và các bằng chứng về việc hệ thống vận hành ổn định.

## Nội dung chính

Mục `5.6` được tổ chức theo các nhóm kết quả thực tế:

1. **Developer push code lên GitHub và trigger CI/CD pipeline** 
![Overview](/images/5-Workshop/Workshop-img/result-1.jpg)
![Overview](/images/5-Workshop/Workshop-img/result-2.jpg)
2. **Gmail thông báo quy trình deploy** 
![Overview](/images/5-Workshop/Workshop-img/result-6.jpg)
3. **Kết quả triển khai trên Amazon ECS**
- Dịch vụ **globalmart-frontend-service** ở trạng thái **Active**, đang chạy **1/1 Task** và triển khai thành công với phiên bản Task Definition mới nhất.
- Dịch vụ **globalmart-backend-service** ở trạng thái **Active**, đang chạy **1/1 Task** và triển khai thành công với phiên bản Task Definition mới nhất.
![Overview](/images/5-Workshop/Workshop-img/result-3.jpg)
![Overview](/images/5-Workshop/Workshop-img/result-4.jpg)
4. **Kết quả website đã cập nhật code mới là chức năng sửa profile** 
![Overview](/images/5-Workshop/Workshop-img/result-5.jpg)
5. **Thông báo cảnh báo qua Email CPU cao ở backend từ Amazone Cloudwatch** 
![Overview](/images/5-Workshop/Workshop-img/result-7.jpg)


## Kết quả mong đợi

Dự án đã được triển khai thành công và vận hành ổn định trên AWS.
