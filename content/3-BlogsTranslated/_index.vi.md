---
title: "Các bài blogs đã dịch"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

###  [Blog 1 - Amazon Bedrock AgentCore: policy controls và quality evaluations cho trusted AI agents](3.1-Blog1/)
Blog này tóm tắt các cập nhật mới của **Amazon Bedrock AgentCore** và tập trung vào 2 mảng quan trọng khi đưa AI agents vào production: **policy controls** (governance/guardrails) để giới hạn hành động/tool call, và **quality evaluations** (continuous monitoring) để theo dõi chất lượng theo thời gian.
###  [Blog 2 - Giảm gian lận SMS OTP với Vonage network-powered solutions và Amazon Cognito](3.2-Blog2/)
Blog này trình bày cách kết hợp **Vonage network-powered solutions** với **Amazon Cognito** thông qua flow `CUSTOM_AUTH` để tăng độ tin cậy của xác thực người dùng và giảm “friction tax” của SMS OTP. Nội dung bao gồm 3 trụ cột **Identity Insights**, **Verify (Silent Authentication + fallback)** và **Fraud Defender**, kèm kiến trúc triển khai với Lambda triggers và luồng authentication “happy path”.
###  [Blog 3 - Cyber resilience on AWS: Reference approach để recovery khỏi ransomware và destructive events](3.3-Blog3/)
Blog này trình bày một reference approach để thiết kế khả năng **cyber recovery** trên AWS: tách trust boundaries giữa **Production account**, **Recovery account** (AWS Backup logically air-gapped vault + MPA) và **Isolated Recovery Environment (IRE)**; xây dựng **validation pipeline** để kiểm tra backup có “safe to use”; cách chọn recovery point an toàn; workflow recovery theo các stage chạy song song; và framework **Rebuild–Restore–Rotate** để quyết định thứ gì rebuild từ IaC, thứ gì restore từ backup và thứ gì phải rotate/re-issue.
###  [Blog 4 - Introducing open-source skills cho AWS SDK best practices](3.4-Blog4/)
Blog này giới thiệu bộ AWS SDK Skills trong dự án open-source **Agent Toolkit for AWS**. Nội dung tập trung vào: vì sao AI agents hay viết code AWS SDK “có vẻ đúng” nhưng sai chi tiết; một skill gồm những gì (`SKILL.md`, `references/`, `scripts/`); các lỗi phổ biến skills giúp tránh (không compile, kém hiệu năng/tốn chi phí, bug khó thấy); cách AWS đo lường hiệu quả; và cách cài các skills như `aws-sdk-swift-usage`, `aws-sdk-js-v3-usage`, `aws-sdk-python-usage`.
###  [Blog 5 - Better Together: Amazon EKS Auto Mode và Istio Ambient Mesh](3.5-Blog5/)
Blog này trình bày cách kết hợp **Amazon EKS Auto Mode** (tự động hoá compute layer: provisioning/scaling/patching) với **Istio Ambient Mesh** (automatic mTLS và policy enforcement mà không bắt buộc sidecar cho mọi pod). Nội dung bao gồm các thành phần chính (Bottlerocket, built-in system components, Karpenter; istio-cni, ztunnel, waypoint, HBONE), sự khác nhau giữa L4 và L7 flows, và outline thực hành deploy bằng Terraform/Helm, kèm kiểm tra mTLS qua Kiali.
###  [Blog 6 - AWS: Vì sao agentic AI là bước ngoặt cho enterprise modernization](3.6-Blog6/)
Blog này phân tích vì sao AWS xem **agentic AI** là bước ngoặt lớn của enterprise modernization. Nội dung tập trung vào mô hình 3 lớp của **AWS Transform** (specialized agents, orchestration, natural-language collaboration), bài toán kinh tế mới của modernization (tăng tốc từ 5x đến 80x), và các kết quả thực tế từ Experian, CSL, BMW Group và Air Canada.
