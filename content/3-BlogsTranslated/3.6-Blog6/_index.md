---
title: "Blog 6"
date: 2026-01-27
weight: 6
chapter: false
pre: " <b> 3.6. </b> "
---
# AWS: Why agentic AI marks an inflection point for enterprise modernization

## Story overview: faster modernization with lower cost

This article frames **agentic AI** as a turning point for enterprise modernization. The core claim is strong: organizations can modernize dramatically faster while reducing cost by using AI agents to tackle technical debt, migration, and application modernization work that traditionally consumes large teams for months.

The business urgency is clear:

- Technical debt in the U.S. is estimated at **$2.41 trillion per year**
- It would require **$1.52 trillion** to fix
- Enterprises that can apply intelligent agents to this problem can reduce costs and gain competitive advantages in time-to-market, business intelligence, and innovation speed

AWS positions **AWS Transform** as the first agentic AI service built specifically for enterprise transformation.

---

## Why this matters now

Enterprise leaders are under pressure to move faster with AI, but they also know that unlocking AI at scale usually starts with **cloud migration and modernization**. Legacy applications, infrastructure, and security configurations create friction that slows down transformation.

The article highlights a practical example: a customer with **6,000 virtual machines** whose VMware licensing was about to expire. Migrating the servers was only part of the challenge. The team also needed to handle:

- Network configurations accumulated over years
- Security policies and permissions
- Dependency mapping across environments

Traditionally, this means weeks of spreadsheet-heavy, error-prone analysis. AWS uses this example to show why a new model is needed.

---

## The three-layer model behind AWS Transform

AWS Transform is presented as a three-layer system, which is what differentiates it from simple automation or a generic chatbot.

### Layer 1: Specialized AI agents

These are purpose-built agents trained for specific transformation tasks, such as:

- Modernizing mainframe languages like **COBOL** into modern languages like **Java**
- Migrating complex VMware networking configurations to AWS
- Upgrading older **.NET** applications to supported framework versions

### Layer 2: Agentic AI coordination system

This layer orchestrates the work. It decides:

- Which expert agents to invoke
- Which tasks can run in parallel
- How to sequence dependencies that would normally require human coordination

In the VMware networking example, AWS says the system coordinated **network discovery**, **security mapping**, and **validation** agents simultaneously, reducing work that usually takes **two weeks** to **eight hours** with higher accuracy.

### Layer 3: Natural language collaboration interface

The third layer provides a natural language interface so technical and business teams can collaborate more easily. This helps bridge the gap between:

- Business stakeholders
- Architects
- Developers

Complex technical decisions become easier to discuss and understand in business terms.

---

## The new economics of modernization

The article argues that agentic AI changes the economics of transformation:

- Projects that once took **12 to 18 months** can now be completed in **weeks**
- AWS reports overall transformation speeds around **5x faster**
- VMware-related migrations can reach **up to 80x acceleration** when multiple tasks run in parallel

This is framed not just as automation, but as a new operating model for modernization.

---

## Customer examples and measured impact

### Experian

Experian needed to modernize seven internal applications running on older .NET frameworks. Using AWS Transform, the company automated migration to **.NET 8** on cloud infrastructure and achieved approximately:

- **40% reduction in developer effort**
- About **300 engineering days saved** across seven projects

The value here is that modernization work happened without pulling developers away from other high-impact projects.

### CSL

CSL, a global biotechnology company, used AWS Transform to accelerate planning across **5,000 servers in 29 data centers**. The article highlights:

- **10x faster planning**
- Application discovery reduced from **hours** to **five minutes**

The business implication is direct: faster modernization supports faster delivery of life-saving medicines to patients.

### BMW Group

BMW Group used AWS Transform for mainframe-related work and achieved:

- **75% reduction in testing time**
- **60% increase in test coverage**

This shows how agentic AI can reduce risk while speeding up modernization.

### Air Canada

Air Canada faced technical debt across thousands of Lambda functions using end-of-life runtimes. Their platform team used AWS Transform to upgrade **Node.js 16 to 20** in a few days, achieving:

- **90% efficacy**
- **80% reduction in expected time and cost**

---

## Modernization as a continuous capability

One of the most important ideas in the article is that modernization should no longer be treated as a one-time project. Frameworks continue to change, security requirements evolve, and business priorities shift. AWS describes a future where adaptation becomes continuous rather than disruptive.

The article also mentions a new **composability** capability, which allows partners to bring:

- Industry-specific data
- Purpose-built agents
- Internal knowledge

This means the system can be extended beyond general transformation use cases into organization-specific modernization patterns.

---

## Human expertise still matters

Even though agentic AI can handle increasingly complex execution work, the article is careful to say that the human role becomes **more important, not less**. The best outcomes come from combining:

- AI execution capabilities
- Human judgment
- Pattern recognition across customer problems
- Business strategy and prioritization

In this model, people focus on strategic decisions while the AI system handles much of the execution.

---

## Why act now

The closing argument is competitive: organizations that adopt agentic AI modernization now can build advantages that compound over time. The opportunity is not only cost reduction, but also freeing resources from legacy maintenance so those resources can be redirected into new AI capabilities.

AWS positions **AWS Transform** as a way to:

- Reduce technical debt
- Accelerate migration and modernization
- Prepare the broader technology stack for AI adoption

The message is that the window for advantage is open now, but won’t stay open forever.

---

## Takeaways for an internship report

- Agentic AI differs from simple automation because it combines specialized agents, orchestration, and natural-language collaboration.
- AWS Transform targets enterprise migration and modernization as a continuous capability, not a one-off project.
- The strongest evidence in the article comes from customer outcomes: faster planning, less developer effort, lower cost, and shorter testing cycles.
- The strategic value is not just modernization speed, but using that speed to unlock broader AI adoption and business competitiveness.
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
