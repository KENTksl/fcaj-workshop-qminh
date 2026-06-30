---
title: "Blog 4"
date: 2026-06-02
weight: 4
chapter: false
pre: " <b> 3.4. </b> "
---
# Introducing open-source skills for AWS SDK best practices

## Problem statement: agents generate plausible SDK code that’s wrong

AI coding agents often know the “general shape” of AWS SDK usage but get the details wrong:

- Incorrect API names or parameter types
- Missing SDK-specific patterns like **paginators** and **waiters**
- Skipping high-level helpers (for example, S3 transfer managers)
- Generating code that looks correct but doesn’t compile, especially for newer SDKs (for example, AWS SDK for Swift and its async patterns)

As developers rely more on agents to generate AWS SDK code, we need a way to help agents consistently produce code that compiles, follows best practices, and uses each SDK as intended.

---

## What’s in a skill

AWS released a set of AWS SDK Skills as part of the open-source **Agent Toolkit for AWS** (Apache-2.0). Skills are modular packages that give coding agents specialized SDK knowledge authored by the SDK team for each language.

A skill includes:

- `SKILL.md`: core instructions, patterns, and concrete examples
- `references/`: deeper documentation loaded only when needed
- `scripts/`: automation for build, test, and validation workflows

Skills are agent-agnostic and work with any coding agent that supports the open skills format.

Repository: https://github.com/aws/agent-toolkit-for-aws

---

## Common mistakes skills help prevent

### 1) Code that doesn’t compile

This is common when the agent’s training data is thin or out of date. Example in Swift (what agents tend to write, which does not compile):

```swift
let client = S3Client()
let response = client.listBuckets(input: ListBucketsInput())
```

With the Swift skill installed, the agent writes the modern Swift concurrency form:

```swift
let config = try await S3Client.S3ClientConfig(region: "us-west-2")
let client = S3Client(config: config)
let response = try await client.listBuckets(input: ListBucketsInput())
```

### 2) Code that runs but performs poorly or costs more

Agents may skip features designed to make calls efficient:

- Paginators for list operations (for example ListObjects)
- Waiters for polling resource states
- High-level transfer methods for large S3 uploads/downloads (multipart, parallelism)

### 3) Code that runs but has subtle bugs

Examples include:

- Manually marshalling DynamoDB attribute values (easy to get wrong on edge cases)
- Catching generic exceptions instead of typed exceptions (retry logic can swallow real failures)

---

## Measuring the impact

AWS evaluates each skill against a benchmark of real SDK tasks (S3 operations, DynamoDB queries, client configuration, presigned URL generation, credential management). Generated code is graded on whether it compiles, passes lint, and does what the task asked for.

Each task runs twice:

1. Without a skill installed
2. With the relevant skill loaded

Across the suite, code generated with a skill installed consistently passes more checks than code generated without one.

---

## Available skills (launch)

| Skill | SDK | What it covers |
| --- | --- | --- |
| `aws-sdk-swift-usage` | AWS SDK for Swift | Async patterns, struct-based config types, client initialization |
| `aws-sdk-js-v3-usage` | AWS SDK for JavaScript v3 | Package structure, client styles, middleware, runtime validation |
| `aws-sdk-python-usage` | Boto3 / botocore | Client vs resource interfaces, paginators, waiters, error handling |

---

## Get started

To install a skill from the Agent Toolkit for AWS:

```bash
npx skills add aws/agent-toolkit-for-aws/skills --skill <skill>
```

Replace `<skill>` with one of:

- `aws-sdk-swift-usage`
- `aws-sdk-js-v3-usage`
- `aws-sdk-python-usage`

You can pass `--skill` multiple times to install more than one.

---

## Takeaways for an internship report

- Skills improve reliability of agent-generated AWS SDK code by encoding SDK team best practices.
- They reduce time lost debugging plausible-looking but incorrect SDK usage.
- They nudge agents toward efficient patterns (paginators, waiters, transfer managers) and safer error handling (typed exceptions, document clients).

## Core Microservice

Provides foundational data and communication layer, including:  
- **Amazon S3** bucket for data  
- **Amazon DynamoDB** for data catalog  
- **AWS Lambda** to write messages into the data lake and catalog  
- **Amazon SNS** topic as the *hub*  
- **Amazon S3** bucket for artifacts such as Lambda code

> Only allow indirect write access to the data lake through a Lambda function → ensures consistency.

---

## Front Door Microservice

- Provides an API Gateway for external REST interaction  
- Authentication & authorization based on **OIDC** via **Amazon Cognito**  
- Self-managed *deduplication* mechanism using DynamoDB instead of SNS FIFO because:  
  1. SNS deduplication TTL is only 5 minutes  
  2. SNS FIFO requires SQS FIFO  
  3. Ability to proactively notify the sender that the message is a duplicate  

---

## Staging ER7 Microservice

- Lambda “trigger” subscribed to the pub/sub hub, filtering messages by attribute  
- Step Functions Express Workflow to convert ER7 → JSON  
- Two Lambdas:  
  1. Fix ER7 formatting (newline, carriage return)  
  2. Parsing logic  
- Result or error is pushed back into the pub/sub hub  

---

## New Features in the Solution

### 1. AWS CloudFormation Cross-Stack References
Example *outputs* in the core microservice:
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
