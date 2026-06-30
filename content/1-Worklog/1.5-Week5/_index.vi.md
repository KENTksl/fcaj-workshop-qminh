---
title: "Worklog Tuần 5"
date: 2026-05-18
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---
### Mục tiêu tuần 5:

* Hoàn thành Module 5 về bảo mật trên AWS, nắm được Mô hình Chia sẻ trách nhiệm (Shared Responsibility Model), các dịch vụ bảo mật chính.
* Hiểu và thực hành IAM (User/Group/Policy/Role/Assume Role/Least Privilege), Amazon Cognito (User Pool/Identity Pool), AWS Organizations (OU/SCP), AWS Identity Center (SSO), AWS KMS, AWS Security Hub.
* Thực hành hands-on theo Module 05-08 về bảo mật AWS.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Học lý thuyết Module 5 về bảo mật AWS và Mô hình Chia sẻ trách nhiệm <br>&emsp; + Triết lý bảo mật AWS: Security of the Cloud vs Security in the Cloud <br>&emsp; + Mô hình Chia sẻ trách nhiệm: trách nhiệm của AWS (hạ tầng toàn cầu) và khách hàng (cấu hình hệ điều hành/tường lửa/IAM/mã hóa/bảo mật ứng dụng) <br>&emsp; + Phân loại trách nhiệm theo nhóm dịch vụ (hạ tầng/quản lý kết hợp/quản lý hoàn toàn) <br>&emsp; + Các dịch vụ bảo mật chính: IAM, Cognito, Organizations & Identity Center, KMS <br>- Học chi tiết IAM: Root Account (best practice: không dùng hàng ngày, kích hoạt MFA), IAM User/Group/Policy (JSON, Explicit Deny, AWS managed/Customer managed/Inline), IAM Role (Assume Role, least privilege), IAM cho dịch vụ AWS (không hard-code key)                                                                                             | 18/05/2026   | 18/05/2026      | - |
| 3   | - Học Amazon Cognito (User Pool/Identity Pool, đăng nhập qua mạng xã hội, phân quyền chi tiết) <br>&emsp; + AWS Organizations (quản lý nhiều tài khoản, OU, Consolidated Billing, SCP) <br>&emsp; + AWS Identity Center (SSO, Identity Source, Permission Sets, portal) <br>&emsp; + AWS KMS (Master Key/Data Key, mã hóa/giải mã, FIPS 140-2 level 2) <br>&emsp; + AWS Security Hub (kiểm tra bảo mật liên tục, tiêu chuẩn AWS/PCI DSS, security score, Pass/Fail)                                                                                         | 19/05/2026   | 19/05/2026      | - |
| 4   | - **Thực hành Module 05-08 (Hands-on)**: <br>&emsp; + **Lab44:** IAM Role & Condition (tạo User/Group admin EC2/RDS, tạo Role với giới hạn theo IP/thời gian) <br>&emsp; + **Lab48:** Cấp quyền cho ứng dụng truy cập dịch vụ AWS với IAM Role (không dùng hard-code access key) <br>&emsp; + **Lab30:** Giới hạn quyền của User với IAM Permission Boundary (giới hạn quyền tối đa, giảm lỗ hổng privilege escalation)                                                                                         | 20/05/2026   | 20/05/2026      | - |
| 5   | - **Thực hành Module 05-08 (tiếp tục)**: <br>&emsp; + **Lab27:** Quản lý tài nguyên bằng Tag và Resource Groups (gán tag cho tài nguyên, dùng Resource Group để quản lý cùng lúc nhiều tài nguyên) <br>&emsp; + **Lab28:** Quản lý truy cập vào dịch vụ EC2 Resource Tag với AWS IAM (áp dụng least privilege với policy và role dựa trên tag) <br>&emsp; + **Lab18:** Bắt đầu với AWS Security Hub (tổng hợp cảnh báo bảo mật, kiểm tra tuân thủ theo tiêu chuẩn AWS/PCI DSS)                                                                                         | 21/05/2026   | 21/05/2026      | - |
| 6   | - **Thực hành Module 05-08 (hoàn thành)**: <br>&emsp; + **Lab12:** Sử dụng AWS IAM Identity Center để quản lý định danh mạnh mẽ (thiết lập SSO cho nhiều AWS account, gán quyền theo group/team, least privilege) <br>&emsp; + **Lab33:** Giới thiệu về AWS Key Management Service (KMS) (tạo Symmetric/Asymmetric CMK, thiết lập key policy, kết hợp CloudTrail để log) <br>&emsp; + **Lab22:** Tối ưu chi phí EC2 với Lambda (tự động bật/tắt máy chủ bằng Lambda, tìm hiểu Saving Plan) <br>&emsp; + Tổng kết toàn bộ nội dung Module 5                                                                                         | 22/05/2026   | 22/05/2026      | - |


### Kết quả đạt được tuần 5:

* Hoàn thành Module 5 và nắm được toàn bộ kiến thức về bảo mật trên AWS:
  * Mô hình Chia sẻ trách nhiệm (Shared Responsibility Model): Security of the Cloud vs Security in the Cloud.
  * IAM: Root Account best practice, User/Group/Policy (JSON, Explicit Deny, 3 loại policy), IAM Role (Assume Role, least privilege), IAM cho dịch vụ AWS.
  * Amazon Cognito: User Pool/Identity Pool, đăng nhập qua mạng xã hội, phân quyền chi tiết.
  * AWS Organizations: quản lý nhiều tài khoản, OU, Consolidated Billing, SCP.
  * AWS Identity Center: SSO, Permission Sets, portal.
  * AWS KMS: Master Key/Data Key, mã hóa/giải mã, FIPS 140-2 level 2.
  * AWS Security Hub: kiểm tra bảo mật liên tục, tiêu chuẩn AWS/PCI DSS.
* Hiểu và phân biệt được trách nhiệm giữa AWS và khách hàng theo loại dịch vụ.
* Thực hành toàn bộ hands-on Module 05-08 về bảo mật AWS:
  * **Lab44**: IAM Role & Condition (tạo User/Group admin EC2/RDS, tạo Role với giới hạn theo IP/thời gian).
  * **Lab48**: Cấp quyền cho ứng dụng truy cập dịch vụ AWS với IAM Role (không dùng hard-code access key).
  * **Lab30**: Giới hạn quyền của User với IAM Permission Boundary (giới hạn quyền tối đa, giảm lỗ hổng privilege escalation).
  * **Lab27**: Quản lý tài nguyên bằng Tag và Resource Groups (gán tag cho tài nguyên, dùng Resource Group để quản lý cùng lúc nhiều tài nguyên).
  * **Lab28**: Quản lý truy cập vào dịch vụ EC2 Resource Tag với AWS IAM (áp dụng least privilege với policy và role dựa trên tag).
  * **Lab18**: Bắt đầu với AWS Security Hub (tổng hợp cảnh báo bảo mật, kiểm tra tuân thủ theo tiêu chuẩn AWS/PCI DSS).
  * **Lab12**: Sử dụng AWS IAM Identity Center để quản lý định danh mạnh mẽ (thiết lập SSO cho nhiều AWS account, gán quyền theo group/team, least privilege).
  * **Lab33**: Giới thiệu về AWS Key Management Service (KMS) (tạo Symmetric/Asymmetric CMK, thiết lập key policy, kết hợp CloudTrail để log).
  * **Lab22**: Tối ưu chi phí EC2 với Lambda (tự động bật/tắt máy chủ bằng Lambda, tìm hiểu Saving Plan).


