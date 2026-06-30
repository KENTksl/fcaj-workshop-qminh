---
title: "Blog 3"
date: 2026-05-20
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# Cyber resilience on AWS: Reference approach để recovery khỏi ransomware và destructive events

## Vấn đề: khôi phục hệ thống về known-good state

Cyber resilience là khả năng khôi phục workload về **known-good state** sau khi môi trường bị adversary tác động. Prevention giúp chặn threat actors, detection giúp phát hiện sớm, nhưng cyber resilience tập trung vào **recovery** trong tình huống backups, credentials hoặc một phần hạ tầng **không còn đáng tin**.

Bài viết này tóm tắt một reference approach với các ý chính:

- Cô lập recovery khỏi production để không phụ thuộc vào production identity khi production bị compromise
- Bảo vệ backup storage khỏi bị xoá bằng **AWS Backup logically air-gapped vault**
- Validate recovery points trước khi restore để giảm rủi ro “restore lại đúng thứ gây sự cố”
- Dùng framework ra quyết định thực tế: **Rebuild–Restore–Rotate**

---

## Cô lập recovery khỏi production (tách trust boundaries)

Ý tưởng cốt lõi là recovery environment (identities, keys, network paths) không nên nằm trong cùng trust boundary với môi trường đang cần recovery. Pattern phổ biến trong **AWS Organizations** dùng 3 account roles:

- **Production account**: nơi workload chạy. Khi incident được xác nhận, account này được cô lập để điều tra. Recovery không nên làm trực tiếp trong production vì remediation in-place đôi khi không khôi phục lại được mức độ tin cậy.
- **Recovery account**: sở hữu AWS Backup **logically air-gapped vault** và enforce deletion protection. Account này kiểm soát restore authorization và có thể yêu cầu **Multi-party approval (MPA)**. Có thể dùng SCP để giới hạn account vào backup-only operations.
- **Isolated Recovery Environment (IRE)**: nơi restore + validate và rebuild “new production” trước khi cutover. IRE không có trust relationship với production, không có VPC peering tới production, và không có internet-facing resources để nếu restore bị “tainted” thì threat không lan ngược về production hay ra internet.

![Account separation and trust boundaries](/images/3-BlogsTranslated/blog3/revblog-1142-images-1.png)

---

## Best practices cho AWS Backup logically air-gapped vault

Cách nhìn đúng là xem vault như “recovery control plane”:

- **Dùng vault cho deletion protection**: logically air-gapped vault luôn ở Compliance mode, nên recovery points không thể bị xoá hoặc rút ngắn retention bởi bất kỳ principal nào (kể cả root) trong retention window.
- **Hiểu nơi recovery points được lưu**: vault object nằm trong Recovery account, nhưng recovery points được lưu trong AWS service-owned accounts; encryption có thể dùng AWS-owned key hoặc AWS KMS customer managed key.
- **Share recovery points để restore**: share qua **AWS RAM** để IRE có thể restore từ Recovery account vault.
- **MPA cho restore**: approval thông qua IAM Identity Center hữu ích khi production accounts không còn trusted.
- **Backup managed resources trực tiếp vào vault**: AWS Backup hỗ trợ write thẳng vào air-gapped vault cho dịch vụ như Amazon S3, Amazon DynamoDB, Amazon EFS.
- **Bổ sung cho các trường hợp S3 đặc biệt**: với data không backup được bằng AWS Backup (ví dụ SSE-C hoặc một số storage classes), dùng **S3 Object Lock (Compliance mode)** + **S3 Versioning**.

---

## Validation pipeline: “restore được” chưa chắc “an toàn”

Restore thành công chỉ chứng minh backup đọc được. Validation nhằm chứng minh backup **safe to use**. Không có một check nào bắt được tất cả, nên nên kết hợp nhiều lớp:

| Layer type | Capability | What it provides |
| --- | --- | --- |
| AWS native | AWS Backup Restore Testing | Tự động verify backup restore được, có custom hooks qua PutRestoreValidationResult |
| AWS native | Amazon GuardDuty Malware Protection | Malware scanning trên restored volumes |
| AWS Partner | Marketplace partner solutions | Content-level ransomware scanning mà không cần full restore trước |
| Workload-specific | Integrity & consistency checks | DB consistency, application invariants, configuration diffs vs known-good baseline |
| Cross-cutting | Log & audit review | Tìm identity/config changes bất thường qua CloudTrail và workload logs |

Validation nên chạy trong **IRE** để nếu fail thì restore bị giữ lại trong IRE và không thể “chạm” về production hoặc internet.

---

## Chọn recovery point an toàn

Trong cyber events, backup mới nhất có thể không an toàn nếu adversary đã hiện diện trước khi bị phát hiện. Cách tiếp cận thực tế:

1. Dựng investigation timeline (CloudTrail, VPC Flow Logs, GuardDuty, Security Hub, workload logs) để xác định earliest plausible indicator.
2. Lấy timestamp đó làm **event boundary**.
3. Duyệt candidates theo thứ tự ngược thời gian, bắt đầu từ backup gần nhất nhưng nằm trước boundary.
4. Chạy validation pipeline; nếu fail thì lùi về candidate cũ hơn.

![Selecting the most recent safe recovery point](/images/3-BlogsTranslated/blog3/figure2.png)

---

## Recovery workflow (các stage có thể chạy song song)

Recovery có 5 stage. Downtime bị quyết định bởi path chậm nhất, nên một số stage nên chạy song song:

1. **Establish the timeline** (investigation)
2. **Validate candidates** (validation pipeline)
3. **Approval** (gate trước restore; thường dùng MPA)
4. **Rebuild and restore** (rebuild infra từ IaC, restore validated data, rotate credentials)
5. **Cutover** (chuyển traffic sang môi trường mới, cập nhật cross-account dependencies)

![Parallel recovery workflow and approval gate](/images/3-BlogsTranslated/blog3/Picture6.jpg)

---

## Rebuild–Restore–Rotate framework

Cyber recovery cần phân loại thứ nào rebuild từ code, thứ nào restore từ backup, và thứ nào phải phát hành mới:

**Infrastructure is code. Data is backup. Credentials are new.**

| Category | Examples | Why |
| --- | --- | --- |
| Rebuild from code | IAM roles/policies, Security Groups, VPC, EC2, Lambda, CI/CD definitions | Ưu tiên IaC đã review và version-controlled thay vì config có thể bị taint |
| Restore from backup | Aurora/EFS/EBS/FSx | Business data không thể tạo lại từ code và phải đến từ immutable backups đã validate |
| Rotate / re-issue | Access keys, DB passwords, API keys, certificates, OAuth tokens, SSH keys | Thay thế secrets có thể đã lộ trong event window |

Một số tài nguyên nằm giữa hai nhóm (ví dụ S3/DynamoDB có cả config và data). Các derived stores (search indexes, caches, analytics tables) thường rebuild từ dữ liệu đã restore và cần được sequence đúng trong runbook.

---

## Next steps (checklist)

- Tạo logically air-gapped vault trong Recovery account và cấu hình MPA.
- Dựng IRE từ trước, không trust relationship và không network path về production; enforce isolation bằng SCPs.
- Lên lịch AWS Backup Restore Testing và bật GuardDuty Malware Protection cho scanning.
- Định nghĩa workload-specific integrity checks (DB consistency, invariants, config diffs).
- Đảm bảo credential rotation đã được “tập” và có thể invoke khi recovery.
- Inventory cross-account dependencies (trust policies, resource policies, KMS grants, integrations) trong recovery runbook.
- Diễn tập end-to-end workflow định kỳ (investigation → validation → rebuild → restore → cutover).

---

## Kết luận rút ra cho báo cáo thực tập

- Recovery phải giả định production identities và cả backups có thể bị nhắm đến.
- Cô lập recovery bằng tách account và IRE để contain tainted restores.
- Validation phải là pipeline (malware + integrity + audit), không phải một check đơn lẻ.
- Dùng Rebuild–Restore–Rotate để quyết định recover từ code vs backup vs phát hành mới.

