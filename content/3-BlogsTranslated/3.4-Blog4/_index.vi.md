---
title: "Blog 4"
date: 2026-06-02
weight: 4
chapter: false
pre: " <b> 3.4. </b> "
---

# Introducing open-source skills cho AWS SDK best practices

## Vấn đề: agent viết code AWS SDK “có vẻ đúng” nhưng sai chi tiết

AI coding agents thường biết “hình dáng tổng quát” của cách dùng AWS SDK, nhưng hay sai ở các chi tiết quan trọng:

- Sai tên API hoặc sai kiểu tham số
- Bỏ qua các pattern đặc trưng của SDK như **paginators** và **waiters**
- Không dùng các high-level APIs (ví dụ transfer manager của Amazon S3)
- Sinh code nhìn hợp lý nhưng không compile, nhất là với SDK mới như AWS SDK for Swift (có async patterns đặc thù)

Khi developer ngày càng dựa vào AI agents để viết code AWS SDK, điều quan trọng là agent phải tạo ra code compile được, đúng best practices, và dùng SDK “đúng cách” theo thiết kế của từng ngôn ngữ.

---

## Skill là gì và gồm những gì

AWS phát hành một bộ AWS SDK Skills trong dự án open-source **Agent Toolkit for AWS** (Apache-2.0). Skills là các package dạng module, cung cấp kiến thức chuyên biệt theo từng AWS SDK/language, được viết bởi chính SDK team phụ trách ngôn ngữ đó.

Một skill thường bao gồm:

- `SKILL.md`: core instructions với các patterns SDK-specific và ví dụ cụ thể
- `references/`: tài liệu chuyên sâu, chỉ load khi cần
- `scripts/`: automation cho build/test/validation workflows

Skills là agent-agnostic (không phụ thuộc một agent cụ thể), miễn là agent hỗ trợ open skills format.

GitHub repository: https://github.com/aws/agent-toolkit-for-aws

---

## Các lỗi phổ biến mà skills giúp tránh

### 1) Code không compile

Với SDK mới (đặc biệt Swift), agent dễ viết code sai về concurrency/async. Ví dụ (agent hay viết, không compile):

```swift
let client = S3Client()
let response = client.listBuckets(input: ListBucketsInput())
```

Khi có Swift skill, agent sẽ viết theo đúng Swift concurrency:

```swift
let config = try await S3Client.S3ClientConfig(region: "us-west-2")
let client = S3Client(config: config)
let response = try await client.listBuckets(input: ListBucketsInput())
```

### 2) Code chạy được nhưng kém hiệu năng hoặc tốn chi phí

Agent hay bỏ qua các tính năng giúp AWS calls hiệu quả:

- Paginators cho các list operations (ví dụ ListObjects)
- Waiters cho polling state của tài nguyên
- High-level transfer methods cho S3 (multipart uploads, parallelism)

### 3) Code chạy được nhưng có bug “khó thấy”

Ví dụ:

- Tự marshal DynamoDB types kiểu `{\"S\": \"value\"}` dễ sai ở edge cases
- Catch exception quá chung thay vì typed exceptions (retry logic có thể nuốt lỗi thật)

---

## Measuring the impact

AWS đánh giá từng skill trên benchmark các tasks thực tế (S3 operations, DynamoDB queries, client config, presigned URL generation, credential management). Output được chấm theo:

- Có compile được không
- Có pass lint không
- Có làm đúng task yêu cầu không (được judge bởi LLM)

Mỗi task chạy 2 lần: không dùng skill và có load skill. Kết quả cho thấy code có skill thường pass nhiều checks hơn.

---

## Các skills có sẵn (tại thời điểm ra mắt)

| Skill | SDK | Nội dung chính |
| --- | --- | --- |
| `aws-sdk-swift-usage` | AWS SDK for Swift | Async patterns, struct-based config types, client initialization |
| `aws-sdk-js-v3-usage` | AWS SDK for JavaScript v3 | Package structure, client styles, middleware, runtime validation |
| `aws-sdk-python-usage` | Boto3 / botocore | Client vs resource interfaces, paginators, waiters, error handling |

---

## Get started

Cài skill từ Agent Toolkit for AWS:

```bash
npx skills add aws/agent-toolkit-for-aws/skills --skill <skill>
```

Thay `<skill>` bằng một trong các lựa chọn:

- `aws-sdk-swift-usage`
- `aws-sdk-js-v3-usage`
- `aws-sdk-python-usage`

Bạn có thể truyền `--skill` nhiều lần để cài nhiều skill.

---

## Kết luận rút ra cho báo cáo thực tập

- Skills giúp tăng độ “đúng” của code AWS SDK do agent sinh ra bằng cách encode best practices từ SDK team.
- Giảm thời gian debug do code nhìn hợp lý nhưng sai (đặc biệt lỗi compile).
- Tăng khả năng agent dùng đúng SDK features: paginators, waiters, transfer methods và error handling chuẩn (typed exceptions, document clients).

---

## Core microservice

Cung cấp dữ liệu nền tảng và lớp truyền thông, gồm:  
- **Amazon S3** bucket cho dữ liệu  
- **Amazon DynamoDB** cho danh mục dữ liệu  
- **AWS Lambda** để ghi message vào data lake và danh mục  
- **Amazon SNS** topic làm *hub*  
- **Amazon S3** bucket cho artifacts như mã Lambda

> Chỉ cho phép truy cập ghi gián tiếp vào data lake qua hàm Lambda → đảm bảo nhất quán.

---

## Front door microservice

- Cung cấp API Gateway để tương tác REST bên ngoài  
- Xác thực & ủy quyền dựa trên **OIDC** thông qua **Amazon Cognito**  
- Cơ chế *deduplication* tự quản lý bằng DynamoDB thay vì SNS FIFO vì:
  1. SNS deduplication TTL chỉ 5 phút
  2. SNS FIFO yêu cầu SQS FIFO
  3. Chủ động báo cho sender biết message là bản sao

---

## Staging ER7 microservice

- Lambda “trigger” đăng ký với pub/sub hub, lọc message theo attribute  
- Step Functions Express Workflow để chuyển ER7 → JSON  
- Hai Lambda:
  1. Sửa format ER7 (newline, carriage return)
  2. Parsing logic  
- Kết quả hoặc lỗi được đẩy lại vào pub/sub hub

---

## Tính năng mới trong giải pháp

### 1. AWS CloudFormation cross-stack references
Ví dụ *outputs* trong core microservice:
```yaml
Outputs:
  Bucket:
    Value: !Ref Bucket
    Export:
      Name: !Sub ${AWS::StackName}-Bucket
  ArtifactBucket:
    Value: !Ref ArtifactBucket
    Export:
      Name: !Sub ${AWS::StackName}-ArtifactBucket
  Topic:
    Value: !Ref Topic
    Export:
      Name: !Sub ${AWS::StackName}-Topic
  Catalog:
    Value: !Ref Catalog
    Export:
      Name: !Sub ${AWS::StackName}-Catalog
  CatalogArn:
    Value: !GetAtt Catalog.Arn
    Export:
      Name: !Sub ${AWS::StackName}-CatalogArn
