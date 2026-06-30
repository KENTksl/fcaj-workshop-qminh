---
title: "Blog 1"
date: 2026-04-21
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---
# Amazon Bedrock AgentCore adds quality evaluations and policy controls for deploying trusted AI agents

After reading an AWS News Blog post about new capabilities in **Amazon Bedrock AgentCore**, I focused on the pieces that feel most “production-ready” to me: adding clear **policy controls** (governance) and setting up **quality evaluations** (continuous monitoring). These are the two areas that usually become pain points when teams move from demos to real deployments.

**Timeline updates**
- Updated on **March 3, 2026**: Policy in AgentCore is generally available.
- Updated on **March 31, 2026**: AgentCore Evaluations is generally available.

---

## Problem statement: scaling agents safely

When agents become more autonomous, they also become harder to deploy confidently:
- They can call tools that access sensitive data.
- They may take actions without appropriate authorization.
- Output quality can fluctuate over time (prompt/tool/data changes).

AgentCore adds guardrails and measurement to address these issues.

---

## Key new capabilities

### 1) Policy in AgentCore (control boundaries for tool calls)

Policy intercepts **AgentCore Gateway** tool calls before they execute. This allows organizations to define:
- Which tools an agent can access (APIs, AWS Lambda functions, MCP servers, third-party tools).
- What actions are permitted.
- Under which conditions (for example identity claims and tool input constraints).

Policy can be applied consistently across teams and agents, independent of the agent framework or foundation model.

### 2) AgentCore Evaluations (real-time quality intelligence)

AgentCore Evaluations continuously monitors agent behavior using:
- Built-in evaluators such as correctness, helpfulness, safety, goal success rate, context relevance, and tool selection accuracy.
- Custom evaluators for business-specific metrics.

Evaluation results are visualized in **Amazon CloudWatch**, where teams can set alarms when quality metrics drop below thresholds.

### 3) Episodic functionality in AgentCore Memory

A long-term memory strategy that helps agents learn from previous interactions by capturing structured “episodes” (context, actions, outcomes) and reusing extracted learnings in similar future tasks.

### 4) Bidirectional streaming in AgentCore Runtime

Supports more natural voice/conversational agents where users and agents can speak simultaneously, including mid-response interruptions and dynamic adaptation.

---

## How Policy works (implementation summary)

1. Create a **policy engine** in the AgentCore console and associate it with one or more gateways.
2. Choose enforcement mode:
   - Enforce (permit/deny tool calls)
   - Log-only (validate rules before production)
3. Author policies using natural language or **Cedar** for fine-grained permissions.

Example policy intent: allow only users with role `refund-agent` to call a refund tool, and only when `amount < 200`.

---

## How Evaluations works (implementation summary)

1. Create an online evaluation in the AgentCore console.
2. Select the data source:
   - AgentCore agent endpoint, or
   - A CloudWatch log group for an external agent
3. Pick evaluators (built-in and/or custom), set sampling rate and filters.
4. Review results in CloudWatch and configure alarms/automation.

---

## Regional availability notes

AgentCore (including Policy preview) is available in: US East (N. Virginia), US West (Oregon), Asia Pacific (Mumbai, Singapore, Sydney, Tokyo), and Europe (Frankfurt, Ireland). AgentCore Evaluations preview was available in: US East (N. Virginia), US West (Oregon), Asia Pacific (Sydney), and Europe (Frankfurt).

---

## Takeaways for an internship report

- Add authorization policies outside the agent’s reasoning loop for safer tool use.
- Monitor agent quality continuously in production, not only during testing.
- Combine observability + evaluation metrics to detect regressions early and respond faster.
