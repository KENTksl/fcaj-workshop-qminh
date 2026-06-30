---
title: "Blog 1"
date: 2026-04-21
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---
# Amazon Bedrock AgentCore: Policy & Evaluations để triển khai AI agent đáng tin cậy

Sau khi đọc một bài AWS News Blog về các cập nhật mới của **Amazon Bedrock AgentCore**, mình tập trung vào những ý mà mình thấy sát thực tế nhất khi đưa AI agent vào production: cần có **policy** để giới hạn hành động/tool call, và cần có **evaluations** để theo dõi chất lượng liên tục theo thời gian.

**Các mốc cập nhật**
- Cập nhật **03/03/2026**: Policy trong AgentCore đã Generally Available.
- Cập nhật **31/03/2026**: AgentCore Evaluations đã Generally Available.

---

## Vấn đề khi scale AI agent

Khi agent ngày càng tự động và có quyền gọi nhiều tools, rủi ro cũng tăng:
- Agent có thể truy cập dữ liệu nhạy cảm không đúng cách.
- Agent có thể thực hiện hành động vượt quyền.
- Quality có thể bị drift theo thời gian khi prompt/tool/data thay đổi.

AgentCore bổ sung guardrails và measurement để giải quyết các vấn đề này.

---

## Những tính năng mới nổi bật

### 1) Policy trong AgentCore (đặt ranh giới cho tool call)

Policy intercept và kiểm tra các **tool call ở AgentCore Gateway** trước khi chạy. Nhờ đó, tổ chức có thể định nghĩa:
- Agent được phép dùng tool nào (API, AWS Lambda, MCP server, dịch vụ bên thứ ba).
- Được phép làm gì với tool đó.
- Trong điều kiện nào (ví dụ dựa trên claim/role trong JWT và điều kiện trên tham số input của tool).

Policy có thể áp dụng nhất quán cho nhiều agent, độc lập với framework hay model.

### 2) AgentCore Evaluations (theo dõi chất lượng theo thời gian thực)

AgentCore Evaluations đánh giá chất lượng dựa trên hành vi thực tế:
- Built-in evaluators (ví dụ correctness, helpfulness, safety, goal success rate, context relevance, tool selection accuracy).
- Custom evaluators theo yêu cầu nghiệp vụ.

Kết quả hiển thị trong **Amazon CloudWatch** cùng observability signals (logs/metrics/traces) để thiết lập cảnh báo khi chất lượng giảm.

### 3) AgentCore Memory (episodic long-term memory)

Chiến lược memory dài hạn dạng “episodic” giúp agent học từ trải nghiệm: ghi lại các episode (ngữ cảnh, hành động, kết quả) và tái sử dụng bài học cho các tình huống tương tự.

### 4) AgentCore Runtime (bidirectional streaming)

Hỗ trợ hội thoại tự nhiên hơn (đặc biệt voice agent) với khả năng người dùng ngắt lời và agent thích ứng ngay, không cần chờ hết lượt.

---

## Policy hoạt động như thế nào (tóm tắt theo dạng báo cáo)

1. Tạo **policy engine** trên AgentCore console và gắn với một hoặc nhiều gateway.
2. Chọn chế độ:
   - Enforce (permit/deny tool call)
   - Log-only (test rule trước khi bật production)
3. Viết policy bằng mô tả ngôn ngữ tự nhiên hoặc bằng **Cedar** để phân quyền chi tiết.

Ví dụ ý tưởng policy: chỉ role `refund-agent` được gọi refund tool và chỉ khi `amount < 200`.

---

## Evaluations hoạt động như thế nào (tóm tắt theo dạng báo cáo)

1. Tạo online evaluation trong AgentCore console.
2. Chọn nguồn dữ liệu:
   - AgentCore agent endpoint, hoặc
   - CloudWatch log group (agent ngoài AgentCore)
3. Chọn evaluator (built-in/custom), cấu hình sampling rate và filter.
4. Theo dõi trong CloudWatch, tạo alarm và automation khi cần.

---

## Ghi chú về vùng khả dụng

AgentCore (bao gồm Policy preview) có ở: US East (N. Virginia), US West (Oregon), Asia Pacific (Mumbai, Singapore, Sydney, Tokyo), Europe (Frankfurt, Ireland). AgentCore Evaluations preview có ở: US East (N. Virginia), US West (Oregon), Asia Pacific (Sydney), Europe (Frankfurt).

---

## Kết luận rút ra cho báo cáo thực tập

- Nên có lớp policy “ngoài vòng suy luận” của agent để kiểm soát tool call.
- Nên theo dõi chất lượng liên tục (production) thay vì chỉ test trước khi deploy.
- Kết hợp observability + evaluation metrics để phát hiện quality regression sớm và phản ứng nhanh.
