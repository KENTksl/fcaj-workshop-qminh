---
title : "Chuẩn bị IAM OIDC"
date : 2026-06-30 
weight : 1
chapter : false
pre : " <b> 5.3.1. </b> "
---

## Mục tiêu

Hoàn thành việc cấu hình toàn bộ tài nguyên đầu vào cho chu trình tự động hóa: thiết lập kho lưu trữ mã nguồn, khởi tạo **IAM OIDC Identity Provider** và **IAM Role** thay thế cho IAM User truyền thống, chuẩn bị các kho chứa Docker Image tập trung và thiết lập các biến môi trường bảo mật trên GitHub Actions.

## Các bước thực hiện chi tiết

### 1. Tạo IAM OIDC Identity Provider trên AWS

Bước này đăng ký GitHub Actions như một **Identity Provider** được AWS tin tưởng — cho phép GitHub xin cấp credentials tạm thời mà không cần Access Key.

1. Truy cập **AWS Management Console** → Tìm kiếm **IAM** → Menu trái chọn **Identity providers** → Chọn **Add provider**.

2. Điền thông tin cấu hình:
   - **Provider type**: Chọn **OpenID Connect**
   - **Provider URL**: Điền `https://token.actions.githubusercontent.com` → Nhấn **Get thumbprint**
   - **Audience**: Điền `sts.amazonaws.com`

3. Nhấn **Add provider**.

![Overview](/images/5-Workshop/Workshop-img/IAM_Provider-1.jpg)
![Overview](/images/5-Workshop/Workshop-img/IAM_Provider-2.jpg)

---

### 2. Tạo IAM Role cho GitHub Actions

1. Truy cập **IAM** → **Roles** → **Create role**.

2. **Trusted entity type**: Chọn **Web identity**.
   - **Identity provider**: Chọn `token.actions.githubusercontent.com`
   - **Audience**: Chọn `sts.amazonaws.com`

3. Nhấn **Next** → **Next** (bỏ qua bước Add permissions, sẽ thêm sau).

4. **Role name**: Điền `github-actions-oidc-role` → Nhấn **Create role**.

![Overview](/images/5-Workshop/Workshop-img/creat-role-1.jpg)
![Overview](/images/5-Workshop/Workshop-img/creat-role-2.jpg)

---

### 3. Cấu hình Trust Policy chính xác

Sau khi tạo role, cần cập nhật Trust Policy để giới hạn chỉ repository của bạn mới được phép assume role này.

1. Mở role **`github-actions-oidc-role`** vừa tạo → Tab **Trust relationships** → **Edit trust policy**.

2. Thay toàn bộ nội dung bằng JSON sau (thay `YOUR-GITHUB-USERNAME` và `YOUR-REPO-NAME` bằng thông tin thực):

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

3. Nhấn **Update policy**.

---

### 4. Gán Permissions Policy cho IAM Role

1. Trong role **`github-actions-oidc-role`** → Tab **Permissions** → **Add permissions** → **Create inline policy** → **JSON**.

2. Dán vào nội dung JSON sau:

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


3. **Policy name**: Điền `github-actions-oidc-role` → Nhấn **Create policy**.
4. Ghi lại **Role ARN**: `arn:aws:iam::YOUR_AWS_ACCOUNT_ID:role/github-actions-oidc-role`

---

### 5. Cấu hình GitHub Actions Secrets


1. Truy cập **GitHub Repository** → **Settings** → **Secrets and variables** → **Actions**.

2. Tab **Secrets** → **New repository secret**, thêm **4 secret**:

| Secret Name | Giá trị |
|---|---|
| `AWS_ROLE_ARN` | `arn:aws:iam::YOUR_AWS_ACCOUNT_ID:role/github-actions-oidc-role`
 |
| `AWS_REGION` | `ap-southeast-1`
 |
| `AWS_ACCOUNT_ID` | `YOUR_AWS_ACCOUNT_ID` |
| `MAIL_PASSWORD` | `YOUR_EMAIL_APP_PASSWORD` |

![Overview](/images/5-Workshop/Workshop-img/github_secret.jpg)

---

## Kết quả mong đợi

Sau khi hoàn thành bài thực hành này, bạn phải đạt được các kết quả sau:

| Thành phần | Trạng thái |
|---|---|
| OIDC Identity Provider | Đã tạo tại IAM → Identity providers |
| IAM Role `github-actions-oidc-role` | Trust Policy giới hạn đúng repo GitHub |
| Permissions Policy | Đủ quyền ECR, ECS, IAM PassRole, SNS |
| GitHub Secret `AWS_ROLE_ARN` | Đã cấu hình đúng Role ARN |
| GitHub workflow `permissions` block | `id-token: write` đã thêm |
