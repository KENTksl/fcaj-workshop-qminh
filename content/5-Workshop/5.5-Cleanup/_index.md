---
title: "Clean Up Resources"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

## Objectives

After completing the workshop, clean up all AWS resources to avoid unnecessary charges and keep your AWS environment organized.

---

## Steps

Delete resources in the following order to avoid dependency issues.

### 1. Disable GitHub Actions (Optional)

If you no longer plan to deploy the application:

- Disable the GitHub Actions workflow.
- Or delete the workflow file under `.github/workflows/`.

---

### 2. Delete Amazon ECS Resources

Open **Amazon ECS**:

- Delete ECS Services:
  - `globalmart-frontend-service`
  - `globalmart-backend-service`
- Wait until all running tasks have stopped.
- Optionally deregister unused Task Definitions.
- Delete the ECS Cluster `globalmart-cluster`.

---

### 3. Delete Application Load Balancers

Open **EC2 → Load Balancers**:

- Delete the Public Application Load Balancer.
- Delete the Internal Application Load Balancer (if applicable).

Then delete:

- Target Groups
- Listeners

---

### 4. Delete API Gateway

Open **Amazon API Gateway**:

- Select the project API.
- Delete the API.

Delete it after removing the API.

---

### 5. Delete Amazon RDS

Open **Amazon RDS**:

- Delete the RDS Proxy.
- Delete the Database Instance.
- Skip creating a Final Snapshot if it is not needed.
- Delete any remaining manual snapshots.

---

### 6. Delete Secrets Manager Resources

Open **AWS Secrets Manager**:

- Delete the database secret.

---

### 7. Delete Amazon ECR

Open **Amazon ECR**:

- Delete all Docker images.
- Delete the repositories:
  - `globalmart-frontend`
  - `globalmart-backend`

---

### 8. Delete Amazon S3

If an S3 bucket was created:

- Empty the bucket.
- Delete the bucket.

---

### 9. Delete CloudWatch and SNS Resources

Open **Amazon CloudWatch**:

- Delete Log Groups.
- Delete Dashboards.
- Delete Alarms.

Open **Amazon SNS**:

- Delete the topic `globalmart-alerts`.

---

### 10. Delete Networking Resources

Open **Amazon VPC** and delete resources in the following order:

- NAT Gateway
- Route Tables (except the main route table)
- Internet Gateway
- Subnets
- Security Groups (except the default group)
- Finally, delete the VPC

---

## Notes

- Always delete resources in the recommended order to avoid dependency errors.
- Check the AWS Billing console after cleanup to ensure that no billable resources remain.
- If you plan to continue using the project, consider stopping services instead of deleting them permanently.

---

## Expected Results

After completing this section:

| Component | Status |
|---|---|
| GitHub Actions | Disabled or removed |
| Amazon ECS | Cluster and Services deleted |
| Application Load Balancer | Deleted |
| Amazon API Gateway | Deleted |
| Amazon RDS | Deleted |
| AWS Secrets Manager | Secrets deleted |
| Amazon ECR | Images and repositories deleted |
| Amazon S3 | Bucket deleted (if used) |
| Amazon CloudWatch | Logs, Dashboards, and Alarms deleted |
| Amazon SNS | Topic deleted |
| Amazon VPC | Networking resources deleted |