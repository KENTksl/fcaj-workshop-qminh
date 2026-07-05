---
title : "Chuẩn bị GitHub Actions CI/CD"
date : 2026-06-30 
weight : 2
chapter : false
pre : " <b> 5.3.2. </b> "
---

## Mục tiêu

Hoàn thành việc cấu hình toàn bộ tài nguyên đầu vào cho chu trình tự động hóa: thiết lập kho lưu trữ mã nguồn, khởi tạo **IAM OIDC Identity Provider** và **IAM Role** thay thế cho IAM User truyền thống, chuẩn bị các kho chứa Docker Image tập trung và thiết lập các biến môi trường bảo mật trên GitHub Actions.

## Các bước thực hiện chi tiết

### 1. Chuẩn bị cấu trúc GitHub Repository

Đảm bảo mã nguồn đã được tổ chức phân tách rõ ràng trong Repository để GitHub Actions có thể định vị chính xác vị trí build:

- Thư mục ứng dụng **Frontend** (chứa `Dockerfile` phục vụ giao diện).

```dockerfile
# Stage 1: Build the application
FROM node:20-alpine AS builder

WORKDIR /app

COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Stage 2: Serve with Nginx
FROM nginx:alpine

COPY --from=builder /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

- Thư mục ứng dụng **Backend** (chứa `Dockerfile` phục vụ ứng dụng Spring Boot).

```dockerfile
# Stage 1: Build the application
FROM maven:3.9-eclipse-temurin-17 AS builder

WORKDIR /app

COPY mvnw ./
COPY .mvn ./.mvn
COPY pom.xml ./
RUN chmod +x mvnw
RUN ./mvnw dependency:go-offline -B

COPY src ./src
RUN ./mvnw clean package -DskipTests

# Stage 2: Run the application
FROM eclipse-temurin:17-jre-alpine

WORKDIR /app
COPY --from=builder /app/target/*.jar app.jar

EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

---

### 2. Tạo workflow GitHub Actions
- Tại thư mục gốc mã nguồn của bạn trên máy Local (hoặc trực tiếp trên GitHub), tạo cấu trúc thư mục theo đúng quy chuẩn của GitHub Actions: `.github/workflows/`.
- Tạo một tệp mới bên trong thư mục này và đặt tên là **`deploy.yml`**.
- Bạn copy toàn bộ đoạn mã kịch bản tối ưu hóa dưới đây để dán vào file `deploy.yml`.

```yml
name: Build and Deploy to ECS

on:
  push:
    branches: [main]

permissions:
  id-token: write
  contents: read

env:
  AWS_REGION: ap-southeast-1
  ECR_FRONTEND: ${{ secrets.AWS_ACCOUNT_ID }}.dkr.ecr.ap-southeast-1.amazonaws.com/globalmart-frontend
  ECR_BACKEND: ${{ secrets.AWS_ACCOUNT_ID }}.dkr.ecr.ap-southeast-1.amazonaws.com/globalmart-backend
  ECS_CLUSTER: globalmart-cluster
  ECS_SERVICE_FRONTEND: globalmart-frontend-service
  ECS_SERVICE_BACKEND: globalmart-backend-service
  SNS_TOPIC_ARN: arn:aws:sns:ap-southeast-1:${{ secrets.AWS_ACCOUNT_ID }}:globalmart-alerts

jobs:
  deploy:
    name: Build and Deploy
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Configure AWS credentials via OIDC
        uses: aws-actions/configure-aws-credentials@v4
        with:
          role-to-assume: ${{ secrets.AWS_ROLE_ARN }}
          aws-region: ${{ env.AWS_REGION }}
          audience: sts.amazonaws.com

      - name: Login to Amazon ECR
        uses: aws-actions/amazon-ecr-login@v2

      - name: Build and push Frontend
        run: |
          IMAGE_TAG=${{ github.sha }}
          docker build -t $ECR_FRONTEND:$IMAGE_TAG -t $ECR_FRONTEND:latest ./globalmart-app
          docker push $ECR_FRONTEND:$IMAGE_TAG
          docker push $ECR_FRONTEND:latest

      - name: Build and push Backend
        run: |
          IMAGE_TAG=${{ github.sha }}
          docker build -t $ECR_BACKEND:$IMAGE_TAG -t $ECR_BACKEND:latest ./globalmart-api
          docker push $ECR_BACKEND:$IMAGE_TAG
          docker push $ECR_BACKEND:latest

      - name: Deploy Frontend to ECS
        run: |
          IMAGE_TAG=${{ github.sha }}
          TASK_DEF=$(aws ecs describe-task-definition \
            --task-definition globalmart-frontend-task \
            --region $AWS_REGION --query 'taskDefinition' --output json)
          NEW_TASK_DEF=$(echo $TASK_DEF | python3 -c "
          import json, sys
          td = json.load(sys.stdin)
          for c in td['containerDefinitions']:
              c['image'] = '$ECR_FRONTEND:$IMAGE_TAG'
          for key in ['taskDefinitionArn','revision','status','requiresAttributes','compatibilities','registeredAt','registeredBy']:
              td.pop(key, None)
          print(json.dumps(td))
          ")
          NEW_TASK_ARN=$(aws ecs register-task-definition \
            --region $AWS_REGION --cli-input-json "$NEW_TASK_DEF" \
            --query 'taskDefinition.taskDefinitionArn' --output text)
          aws ecs update-service \
            --cluster $ECS_CLUSTER --service $ECS_SERVICE_FRONTEND \
            --task-definition $NEW_TASK_ARN --force-new-deployment \
            --region $AWS_REGION
          echo "Frontend deployed: $IMAGE_TAG"

      - name: Deploy Backend to ECS
        run: |
          IMAGE_TAG=${{ github.sha }}
          TASK_DEF=$(aws ecs describe-task-definition \
            --task-definition globalmart-backend-task \
            --region $AWS_REGION --query 'taskDefinition' --output json)
          NEW_TASK_DEF=$(echo $TASK_DEF | python3 -c "
          import json, sys
          td = json.load(sys.stdin)
          for c in td['containerDefinitions']:
              c['image'] = '$ECR_BACKEND:$IMAGE_TAG'
          for key in ['taskDefinitionArn','revision','status','requiresAttributes','compatibilities','registeredAt','registeredBy']:
              td.pop(key, None)
          print(json.dumps(td))
          ")
          NEW_TASK_ARN=$(aws ecs register-task-definition \
            --region $AWS_REGION --cli-input-json "$NEW_TASK_DEF" \
            --query 'taskDefinition.taskDefinitionArn' --output text)
          aws ecs update-service \
            --cluster $ECS_CLUSTER --service $ECS_SERVICE_BACKEND \
            --task-definition $NEW_TASK_ARN --force-new-deployment \
            --region $AWS_REGION
          echo "Backend deployed: $IMAGE_TAG"

      - name: Notify Success via SNS
        if: success()
        run: |
          aws sns publish \
            --topic-arn $SNS_TOPIC_ARN \
            --subject "GlobalMart Deploy SUCCESS" \
            --message "Deploy thanh cong!
          Repository: ${{ github.repository }}
          Branch: ${{ github.ref_name }}
          Commit: ${{ github.sha }}
          Message: ${{ github.event.head_commit.message }}
          Author: ${{ github.event.head_commit.author.name }}
          Thoi gian: $(date '+%Y-%m-%d %H:%M:%S UTC')
          Xem chi tiet: ${{ github.server_url }}/${{ github.repository }}/actions/runs/${{ github.run_id }}" \
            --region $AWS_REGION

      - name: Notify Failure via SNS
        if: failure()
        run: |
          aws sns publish \
            --topic-arn $SNS_TOPIC_ARN \
            --subject "GlobalMart Deploy FAILED" \
            --message "Deploy THAT BAI! Can kiem tra ngay
          Repository: ${{ github.repository }}
          Branch: ${{ github.ref_name }}
          Commit: ${{ github.sha }}
          Message: ${{ github.event.head_commit.message }}
          Author: ${{ github.event.head_commit.author.name }}
          Thoi gian: $(date '+%Y-%m-%d %H:%M:%S UTC')
          Xem loi tai: ${{ github.server_url }}/${{ github.repository }}/actions/runs/${{ github.run_id }}" \
            --region $AWS_REGION
```


## Kết quả mong đợi

Sau khi hoàn thành bài thực hành này, Repository GitHub đã được chuẩn bị đầy đủ để sẵn sàng triển khai CI/CD với GitHub Actions.

| Thành phần | Trạng thái |
|---|---|
| Cấu trúc Repository | Đã phân tách thư mục Frontend và Backend |
| Dockerfile Frontend | Đã cấu hình để build React và chạy bằng Nginx |
| Dockerfile Backend | Đã cấu hình để build và chạy ứng dụng Spring Boot |
| Thư mục `.github/workflows/` | Đã được tạo |
| Workflow `deploy.yml` | Đã được thêm vào Repository |
| Trigger Workflow | Tự động chạy khi có `push` lên nhánh `main` |
| Permissions | Đã khai báo `id-token: write` và `contents: read` |
| Pipeline CI/CD | Đã định nghĩa đầy đủ các bước Build, Push Image, Deploy ECS và gửi thông báo SNS |
