---
title: "Week 11 Worklog"
date: 2026-06-29
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Week 11 Objective:

- Solve the connectivity problem between `Frontend (React SPA)` and the `Backend` hosted in a private subnet using `API Gateway` and `VPC Link`.
- Set up a `CI/CD pipeline` to automatically build and deploy the application to ECS using `GitHub Actions`.
- Implement `Monitoring & Observability` with `CloudWatch`, `SNS`, and automated alerts via email/SMS.
- Set up `Recovery & Backup` using `RDS Snapshot` and export snapshots to `S3`.

### Weekly Schedule:

| Day | Tasks                                                                                                                                                                                                                                                                                                                                                                                                                                                          | Start Date | Completion Date | References                                            |
| --- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ----------------------------------------------------- |
| 2   | - Analyze the connectivity challenge: `React SPA` (running in browser) to Backend in a private subnet <br> - Choose `API Gateway + VPC Link` instead of exposing Backend publicly <br> - Create `VPC Link` (`vpclink-globalmart`) connected to the `ALB internal` <br> - Create `HTTP API Gateway` (`globalmart-api-gateway`) with Private resource integration                                                                                                | 29/06/2026 | 29/06/2026      |   |
| 3   | - Configure API Gateway `Route`: `ANY /{proxy+}` → Private integration <br> - Deploy API Gateway to a stage (`$default` or `prod`) <br> - Connectivity test: `curl <invoke-url>/actuator/health` returns `{\"status\":\"UP\"}` <br> - Update `REACT_APP_API_URL` in Frontend to point to the correct Invoke URL <br> - Rebuild Frontend image, push to ECR, and redeploy ECS Frontend Service                                                                  | 30/06/2026 | 30/06/2026      |   |
| 4   | - Set up `CI/CD pipeline` using `GitHub Actions` (replace CodePipeline due to CodeBuild quota limits) <br>&emsp; + Create `IAM Role` with OIDC for GitHub Actions to authenticate to AWS <br>&emsp; + Configure `GitHub Secrets`: `AWS_ROLE_ARN`, `AWS_REGION`, `ECR_REPOSITORY`, `ECS_SERVICE`, `ECS_CLUSTER`, `CONTAINER_NAME` <br>&emsp; + Write workflow `.github/workflows/deploy-backend.yml`: build image → push ECR → update ECS Service               | 01/07/2026 | 01/07/2026      |   |
| 5   | - Create `S3 Buckets`: <br>&emsp; + `globalmart-backup-bucket` to store RDS snapshot exports <br> - Configure `Bucket Policy` to allow the RDS export service to write to the backup bucket <br> - Create an `IAM Role` for the RDS export task <br> - Export an `RDS Snapshot` to S3 to demonstrate the Recovery & Backup flow                                                                                                                                | 02/07/2026 | 02/07/2026      |   |
| 6   | - Implement `Monitoring & Observability`: <br>&emsp; + Verify `CloudWatch Logs` receives logs from ECS Frontend and Backend <br>&emsp; + Create `CloudWatch Alarms`: CPU > 80%, ALB 5XX errors > 5 within 5 minutes <br>&emsp; + Create `SNS Topic` (`globalmart-alerts`) and subscribe Email/SMS <br>&emsp; + Attach Alarm Actions → SNS Topic and test alert emails <br> - Finalize and update the overall architecture diagram to match the deployed system | 03/07/2026 | 03/07/2026      |   |

### Results:

- Successfully solved connectivity between `React SPA` and a Backend in a private subnet:
  - Implemented `API Gateway (HTTP API)` + `VPC Link` as a bridge from the Internet into the VPC via `ALB internal` without exposing the Backend publicly.
  - Verified the request flow: Browser → API Gateway → VPC Link → ALB internal → ECS Backend → RDS MySQL.

- Successfully set up a `CI/CD pipeline` using `GitHub Actions`:
  - Workflow triggers automatically on pushes to the `main` branch.
  - Steps: Checkout → Configure AWS credentials (OIDC) → Login to ECR → Build & Push image → Render Task Definition → Deploy ECS Service.
  - ECS Service performs `deployment` (ECS Managed) after receiving updates from GitHub Actions.

- Successfully implemented `Recovery & Backup`:
  - Enabled `RDS Automated Backup` (7-day retention).
  - Exported RDS snapshots to the `S3 Backup Bucket`.
  - Understood the flow: RDS Snapshot → Export → S3 (Snapshots/Exports).

- Successfully implemented `Monitoring & Observability`:
  - `CloudWatch Logs` collects realtime logs from both ECS services.
  - `CloudWatch Alarms` trigger when CPU exceeds threshold or ALB returns 5XX errors.
  - `SNS` sends alerts automatically via Email/SMS.

- Completed the overall `GlobalMart` AWS architecture with 5 major blocks:
  - **Networking**: VPC, Subnet, IGW, NAT Gateway, Security Group.
  - **Deployment (Runtime)**: ECS Fargate, ALB, API Gateway, VPC Link, RDS.
  - **CI/CD**: GitHub Actions, ECR, S3 Artifact Bucket.
  - **Recovery & Backup**: RDS Snapshot, S3 Backup Bucket.
  - **Monitoring & Observability**: CloudWatch Logs, CloudWatch Metrics, CloudWatch Alarms, SNS, Email/SMS.
