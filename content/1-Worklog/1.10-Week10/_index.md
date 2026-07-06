---
title: "Week 10 Worklog"
date: 2026-06-22
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Week 10 Objective:

- Design and deploy networking infrastructure (VPC, Subnet, Security Group, NAT Gateway, IGW) for the `GlobalMart` project on AWS.
- Deploy `RDS for MySQL` (Single-AZ) in a private subnet.
- Package `Frontend` and `Backend` into Docker images and push them to `Amazon ECR`.
- Set up `Application Load Balancer` (internet-facing and internal) and deploy an `ECS Cluster` with `Fargate` for both services.

### Weekly Schedule:

| Day | Tasks                                                                                                                                                                                                                                                                                                                                                                                                                                                         | Start Date | Completion Date | References |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ---------- |
| 2   | - Design the overall `GlobalMart` architecture on AWS <br> - Create `VPC` (`globalmart-vpc`, CIDR `10.0.0.0/16`) <br>&emsp; + Create `Public subnet`, `Private subnet A` (ECS), `Private subnet B` (RDS) <br>&emsp; + Create `Internet Gateway (IGW)` and `NAT Gateway` <br>&emsp; + Configure `Route Table` for each subnet                                                                                                                                  | 22/06/2026 | 22/06/2026      |            |
| 3   | - Create `Security Group` rules for each layer: <br>&emsp; + `sg-alb-public` for internet-facing ALB <br>&emsp; + `sg-ecs-frontend` for ECS Frontend task <br>&emsp; + `sg-ecs-backend` for ECS Backend task <br>&emsp; + `sg-alb-internal` for internal ALB <br>&emsp; + `sg-rds` for RDS MySQL <br> - Create `RDS for MySQL` (Single-AZ, `db.t3.micro`) in `Private subnet B`                                                                               | 23/06/2026 | 23/06/2026      |            |
| 4   | - Create `ECR Repository` for `globalmart-frontend` and `globalmart-backend` <br> - Build Docker images for Frontend (`React/Vite`) and Backend (`Spring Boot`) <br> - Push images to `Amazon ECR` <br> - Create database `globalmart` inside the RDS instance via a temporary EC2 host                                                                                                                                                                       | 24/06/2026 | 24/06/2026      |            |
| 5   | - Create internet-facing `Application Load Balancer` (Public subnet) <br>&emsp; + Create `Target Group tg-frontend` (type IP, port 80) <br>&emsp; + Configure Listener HTTP:80 → forward to `tg-frontend` <br> - Create internal `Application Load Balancer` (Private subnet A) <br>&emsp; + Create `Target Group tg-backend` (type IP, port 8080) <br>&emsp; + Configure Health check path `/actuator/health`                                                | 25/06/2026 | 25/06/2026      |            |
| 6   | - Create `ECS Cluster` (`globalmart-cluster`, Fargate) <br> - Create `Task Definition` for Frontend and Backend <br>&emsp; + Configure CPU, Memory, Container port, Environment variables, CloudWatch Logs <br> - Create `ECS Service Frontend` attached to internet-facing ALB → `tg-frontend` <br> - Create `ECS Service Backend` attached to internal ALB → `tg-backend` <br> - Fix issues: Service-Linked Role, Health check grace period, Security Group | 26/06/2026 | 26/06/2026      |            |

### Results:

- Successfully designed and deployed AWS networking infrastructure:
  - `VPC globalmart-vpc` with CIDR `10.0.0.0/16`.
  - Proper 3-tier subnet design: Public subnet (ALB, NAT), Private subnet A (ECS), Private subnet B (RDS).
  - `Internet Gateway` and `NAT Gateway` configured with correct route tables for each subnet.

- Successfully deployed `RDS for MySQL` (Single-AZ) in a private subnet:
  - No public access; only allow traffic from `sg-ecs-backend` on port 3306.
  - Created database `globalmart` and verified backend connectivity.

- Successfully built and pushed Docker images to `Amazon ECR`:
  - `globalmart-frontend` image for the React/Vite app.
  - `globalmart-backend` image for the Spring Boot API.

- Successfully deployed `ECS Cluster` with `Fargate`:
  - `ECS Service Frontend` runs stably and receives traffic via the internet-facing ALB.
  - `ECS Service Backend` connects to RDS and serves APIs behind the internal ALB.
  - Correct Health check (`/actuator/health`) and Health check grace period (`120 seconds`) to avoid restart loops.

- Understood the traffic isolation model with Security Groups:
  - ALB public → ECS Frontend → ALB internal → ECS Backend → RDS.
  - Each layer only allows the required traffic sources, without unnecessary exposure.
