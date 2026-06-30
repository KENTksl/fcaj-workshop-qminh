---
title: "Translated Blogs"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

This section lists and introduces the blogs you have translated.

###  [Blog 1 - Amazon Bedrock AgentCore adds quality evaluations and policy controls for deploying trusted AI agents](3.1-Blog1/)
This blog summarizes key updates in **Amazon Bedrock AgentCore** and focuses on two production-ready pillars when deploying AI agents: **policy controls** (governance/guardrails) and **quality evaluations** (continuous monitoring over time).

###  [Blog 2 - Reducing SMS OTP fraud with Vonage network-powered solutions and Amazon Cognito](3.2-Blog2/)
This blog explains how **Vonage network-powered solutions** integrate with **Amazon Cognito** via the `CUSTOM_AUTH` flow to strengthen identity verification while reducing the friction of SMS OTP. It covers **Identity Insights**, **Verify (Silent Authentication + fallback)**, and **Fraud Defender**, along with reference architecture and the authentication “happy path”.

###  [Blog 3 - Cyber resilience on AWS: A reference approach for recovery from ransomware and destructive events](3.3-Blog3/)
This blog summarizes a reference approach for cyber recovery on AWS: separating trust boundaries across a Production account, a Recovery account (AWS Backup logically air-gapped vault + MPA), and an Isolated Recovery Environment (IRE); building a validation pipeline to prove backups are safe to use; selecting a safe recovery point; running a parallelizable recovery workflow; and applying the **Rebuild–Restore–Rotate** framework.

###  [Blog 4 - Introducing open-source skills for AWS SDK best practices](3.4-Blog4/)
This blog introduces the open-source AWS SDK Skills in the **Agent Toolkit for AWS**. It explains why agents often generate plausible but incorrect SDK code, what a skill package contains, which mistakes skills help prevent (compile errors, inefficiency, subtle bugs), how AWS measures impact, and how to install skills like `aws-sdk-swift-usage`, `aws-sdk-js-v3-usage`, and `aws-sdk-python-usage`.

###  [Blog 5 - Better Together: Amazon EKS Auto Mode and Istio Ambient Mesh](3.5-Blog5/)
This blog explains how Amazon EKS Auto Mode (compute lifecycle automation) and Istio Ambient Mesh (mTLS and policy enforcement without default sidecars) work together for microservices. It covers key components (Bottlerocket, built-in system components, Karpenter scaling; istio-cni, ztunnel, waypoints, HBONE), L4 vs L7 traffic flows, and a hands-on implementation outline using Terraform, Helm, and Kiali.

###  [Blog 6 - AWS: Why agentic AI marks an inflection point for enterprise modernization](3.6-Blog6/)
This blog explains why AWS sees **agentic AI** as a major turning point for enterprise modernization. It covers the three-layer architecture behind **AWS Transform** (specialized agents, orchestration, natural-language collaboration), the new economics of modernization (5x to 80x acceleration), and customer results from Experian, CSL, BMW Group, and Air Canada.
