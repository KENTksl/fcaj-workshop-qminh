---
title: "Blog 2"
date: 2026-06-17
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# Giảm gian lận SMS OTP với Vonage network-powered solutions và Amazon Cognito

## Vấn đề: OTP fraud và rớt người dùng ở bước xác thực

Xác thực người dùng (user authentication) là một trong những điểm bị nhắm đến nhiều nhất trong bảo mật ứng dụng. Khi gian lận được “công nghiệp hoá” nhờ generative AI, chi phí tội phạm mạng được dự báo có thể đạt **$23 nghìn tỷ** vào năm 2027 (**+175%** so với 2022). **20%** gian lận được cho là đến từ **synthetic identity** và các khai thác liên quan đến authentication, với account takeover (ATO) tăng **141%** kể từ 2021.

SMS One-time passcodes (OTPs) tạo friction: chỉ ~**80% conversion**, nghĩa là **1/5 người dùng hợp lệ rơi rớt** ở bước verify. Ngoài ra còn có chi phí vận hành (support/helpdesk tickets) liên quan đến password recovery và OTP-based verification.

---

## Tổng quan giải pháp: Vonage network-powered solutions + Cognito `CUSTOM_AUTH`

Giải pháp tích hợp Vonage với Amazon Cognito thông qua flow `CUSTOM_AUTH` để:

- Dùng tín hiệu rủi ro từ nhà mạng (real-time) trước khi gửi OTP
- Ưu tiên **Silent Authentication** (zero-tap), giảm “friction tax”
- Fallback tự động sang SMS/RCS/Voice/WhatsApp/email khi cần
- Chặn các hành vi abuse kênh verify như SMS pumping/AIT

“Network-powered” nghĩa là tín hiệu đến từ **real-time data trực tiếp từ mobile network operators (MNOs)**. Điều này quan trọng vì với SIM swap–driven ATO, “recently” có thể chỉ là vài phút/giờ; database tĩnh cập nhật theo tuần thường phát hiện quá trễ.

---

## Các thành phần chính (3 trụ cột)

### 1) Identity Insights (pre-verification)

Chạy trước khi initiate kênh verification để ra quyết định policy:

- `format`, `network_type`: lọc số invalid/VoIP/landline/premium-rate numbers
- `sim_swap`: phát hiện SIM swap trong look-back window
- `subscriber_match`: đối chiếu thông tin subscriber với KYC records của nhà mạng

Kết quả có thể là step-up challenge, hard block hoặc silent logging — trước khi gửi OTP.

### 2) Verify (Silent Authentication + fallback)

Ưu tiên **Silent Authentication** (proof of possession = cellular data session). Nếu không khả dụng, hệ thống fallback sang SMS/RCS/Voice/WhatsApp/email một cách minh bạch với người dùng.

### 3) Fraud Defender (bảo vệ kênh verify)

Theo dõi và chặn abuse như SMS pumping và artificially inflated traffic (AIT) trước khi chi phí phát sinh lớn.

---

## Kiến trúc với Amazon Cognito

Tích hợp dùng Amazon Cognito `CUSTOM_AUTH` với 3 Lambda triggers:

- Define Auth Challenge (orchestrator)
- Create Auth Challenge (gọi Vonage APIs)
- Verify Auth Challenge (validate)

![Tong quan risk-adaptive sign-in](/images/3-BlogsTranslated/blog2/ARCHBLOG-1533-1.png)

---

## Authentication flow (happy path)

1. Client gọi `InitiateAuth` với `CUSTOM_AUTH`, truyền phone number.
2. Cognito issue `CUSTOM_CHALLENGE`.
3. Create Auth Challenge gọi Identity Insights; nếu pass thì initiate Verify Silent Auth và trả `check_url`.
4. Client mở `check_url` qua cellular data; nhà mạng xác minh và trả verification code.
5. Client gọi `RespondToAuthChallenge` kèm code.
6. Verify Auth Challenge validate với Vonage; nếu thành công thì Cognito issue tokens.

![User login happy path sequence](/images/3-BlogsTranslated/blog2/ARCHBLOG-1533-2.png)

---

## Lưu ý triển khai (AWS)

- Lưu Vonage credentials trong AWS Secrets Manager
- IAM least privilege cho Lambda roles
- CloudWatch Logs cho từng Lambda trigger
- CloudTrail cho audit trails và AWS WAF rate-limiting chống brute-force
- Enforce TLS 1.2+ (encryption in transit)

---

## Production outcomes (ví dụ)

Lydia Solutions ghi nhận:

- Giảm đến **50% latency** so với dịch vụ xác thực trước đó
- Kết quả phổ biến ở nhiều triển khai: cải thiện conversion **2–8.5%** so với SMS-only và latency giảm **50–75%**

---

## Kết luận rút ra cho báo cáo thực tập

- Dùng risk signals để quyết định khi nào cần friction (step-up) vs silent verification.
- Ưu tiên network-level proof (Silent Auth) để giảm các vector tấn công OTP.
- Xem verification cost như một metric song hành giữa security và business (conversion + SMS pumping).
