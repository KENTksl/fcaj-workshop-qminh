---
title : "Prepare IAM OIDC"
date : 2026-06-30 
weight : 1
chapter : false
pre : " <b> 5.3.1. </b> "
---

## Objectives

Complete the configuration of all input resources for the automation pipeline: set up source code repository, initialize **IAM OIDC Identity Provider** and **IAM Role** to replace traditional IAM User, prepare centralized Docker Image repositories, and set up secure environment variables on GitHub Actions.

## Detailed implementation steps

### 1. Create IAM OIDC Identity Provider on AWS

This step registers GitHub Actions as an **Identity Provider** trusted by AWS — allowing GitHub to request temporary credentials without needing an Access Key.

1. Access **AWS Management Console** → Search for **IAM** → Left menu select **Identity providers** → Choose **Add provider**.

2. Fill in the configuration information:
   - **Provider type**: Select **OpenID Connect**
   - **Provider URL**: Enter `https://token.actions.githubusercontent.com` → Click **Get thumbprint**
   - **Audience**: Enter `sts.amazonaws.com`

3. Click **Add provider**.

![Overview](/images/5-Workshop/Workshop-img/IAM_Provider-1.jpg)
![Overview](/images/5-Workshop/Workshop-img/IAM_Provider-2.jpg)

---

### 2. Create IAM Role for GitHub Actions

1. Access **IAM** → **Roles** → **Create role**.

2. **Trusted entity type**: Select **Web identity**.
   - **Identity provider**: Select `token.actions.githubusercontent.com`
   - **Audience**: Select `sts.amazonaws.com`

3. Click **Next** → **Next** (skip Add permissions step, will add later).

4. **Role name**: Enter `github-actions-oidc-role` → Click **Create role**.

![Overview](/images/5-Workshop/Workshop-img/creat-role-1.jpg)
![Overview](/images/5-Workshop/Workshop-img/creat-role-2.jpg)

---

### 3. Configure Trust Policy correctly

After creating the role, we need to update the Trust Policy to restrict only your repository to be able to assume this role.

1. Open the **`github-actions-oidc-role`** you just created → **Trust relationships** tab → **Edit trust policy**.

2. Replace the entire content with the following JSON (replace `YOUR-GITHUB-USERNAME` and `YOUR-REPO-NAME` with your actual information):

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Federated": "arn:aws:iam::YOUR_AWS_ACCOUNT_ID:oidc-provider/token.actions.githubusercontent.com"
      },
      "Action": "sts:AssumeRoleWithWebIdentity",
      "Condition": {
        "StringEquals": {
          "token.actions.githubusercontent.com:aud": "sts.amazonaws.com",
          "token.actions.githubusercontent.com:sub": "repo:YOUR-GITHUB-USERNAME/YOUR-REPO-NAME:ref:refs/heads/main"
        }
      }
    }
  ]
}
```
![Overview](/images/5-Workshop/Workshop-img/IAM_Trust-1.jpg)
![Overview](/images/5-Workshop/Workshop-img/IAM_Trust-2.jpg)

3. Click **Update policy**.

---

### 4. Attach Permissions Policy to IAM Role

1. In the **`github-actions-oidc-role`** role → **Permissions** tab → **Add permissions** → **Create inline policy** → **JSON**.

2. Paste the following JSON content:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowECRAuth",
      "Effect": "Allow",
      "Action": [
        "ecr:GetAuthorizationToken"
      ],
      "Resource": "*"
    },
    {
      "Sid": "AllowECRProjectActions",
      "Effect": "Allow",
      "Action": [
        "ecr:BatchCheckLayerAvailability",
        "ecr:GetDownloadUrlForLayer",
        "ecr:BatchGetImage",
        "ecr:InitiateLayerUpload",
        "ecr:UploadLayerPart",
        "ecr:CompleteLayerUpload",
        "ecr:PutImage"
      ],
      "Resource": "arn:aws:ecr:*:YOUR_AWS_ACCOUNT_ID:repository/*"
    },
    {
      "Sid": "AllowECSAdvancedDeployment",
      "Effect": "Allow",
      "Action": [
        "ecs:DescribeTaskDefinition",
        "ecs:RegisterTaskDefinition",
        "ecs:UpdateService",
        "ecs:DescribeServices"
      ],
      "Resource": "*"
    },
    {
      "Sid": "AllowPassRole",
      "Effect": "Allow",
      "Action": [
        "iam:PassRole"
      ],
      "Resource": "arn:aws:iam::YOUR_AWS_ACCOUNT_ID:role/*"
    },
    {
      "Sid": "AllowSNSPublish",
      "Effect": "Allow",
      "Action": [
        "sns:Publish"
      ],
      "Resource": "arn:aws:sns:ap-southeast-1:YOUR_AWS_ACCOUNT_ID:globalmart-alerts"
    }
  ]
}
```
![Overview](/images/5-Workshop/Workshop-img/IAM_Role-1.jpg)
![Overview](/images/5-Workshop/Workshop-img/IAM_Role-2.jpg)  
![Overview](/images/5-Workshop/Workshop-img/IAM_Role-3.jpg) 


3. **Policy name**: Enter `github-actions-oidc-role` → Click **Create policy**.
4. Note down the **Role ARN**: `arn:aws:iam::YOUR_AWS_ACCOUNT_ID:role/github-actions-oidc-role`

--

### 5. Configure GitHub Actions Secrets

With OIDC, we **no longer store Access Keys** — only the Role ARN is needed.

1. Access **GitHub Repository** → **Settings** → **Secrets and variables** → **Actions**.

2. **Secrets** tab → **New repository secret**, add **4 secrets**:

| Secret Name | Value |
|---|---|
| `AWS_ROLE_ARN` | `arn:aws:iam::YOUR_AWS_ACCOUNT_ID:role/github-actions-oidc-role` |
| `AWS_REGION` | `ap-southeast-1` |
| `AWS_ACCOUNT_ID` | `YOUR_AWS_ACCOUNT_ID` |
| `MAIL_PASSWORD` | `YOUR_EMAIL_APP_PASSWORD` |

![Overview](/images/5-Workshop/Workshop-img/github_secret.jpg)

---

## Expected results

After completing this lab, you must achieve the following results:

| Component | Status |
|---|---|
| OIDC Identity Provider | Created at IAM → Identity providers |
| IAM Role `github-actions-oidc-role` | Trust Policy correctly restricted to GitHub repo |
| Permissions Policy | Sufficient permissions for ECR, ECS, IAM PassRole, SNS |
| ECR `globalmart-frontend` | Scan on push: Enabled |
| ECR `globalmart-backend` | Scan on push: Enabled |
| GitHub Secret `AWS_ROLE_ARN` | Correct Role ARN configured |
| GitHub workflow `permissions` block | `id-token: write` added |

> **Security Note**: With OIDC, no Access Keys are created or stored. Every time the workflow runs, AWS issues a temporary token that only exists during the job execution — even if leaked, it cannot be reused.
