---
title : "Deploy ECS, ALB, API Gateway, and VPC Link"
date : 2024-01-01
weight : 2
chapter : false
pre : " <b> 5.4.2 </b> "
---

## Objectives

Bring frontend and backend into the runtime environment, while configuring request reception and routing layers according to the project architecture.

## Implementation steps

1. **Create ECS Cluster and task definitions**
   - Create ECS Cluster using AWS Fargate.
   - Create separate task definitions for frontend and backend.
   - Configure CPU, memory, port mapping, logging, and basic environment variables.

2. **Deploy ECS services**
   - Create services for frontend and backend in private subnets.
   - Configure desired count and autoscaling if you use it in your project.

3. **Create Public ALB**
   - Create Internet-facing ALB to receive external requests.
   - Attach appropriate target group for frontend service.
   - Configure listener, health check, and routing rules if needed.

4. **Create Internal ALB**
   - Create Internal ALB to distribute requests to backend service in VPC.
   - Attach backend target group and configure internal listener.

5. **Create API Gateway and VPC Link**
   - Create API Gateway as entry point for backend.
   - Create VPC Link to connect API Gateway with Internal ALB.
   - Configure `/api` route or business routes appropriate for your application.

## Desired access flow

- Users access the system through Public ALB.
- Frontend is served from the front ECS service.
- Frontend or client calls API through API Gateway.
- API Gateway uses VPC Link to access Internal ALB.
- Internal ALB forwards requests to backend ECS service.

## Expected results

After this step, frontend and backend are both running on ECS and have a clear request path from the Internet to the business logic layer.

## Suggested images to add

- ECS Cluster and ECS services.
- Frontend and backend task definitions.
- Public ALB and target group.
- API Gateway, VPC Link, and Internal ALB.
