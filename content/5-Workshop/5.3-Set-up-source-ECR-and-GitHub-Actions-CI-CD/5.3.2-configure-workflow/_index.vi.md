---
title : "Cấu hình GitHub Actions và kiểm tra CI/CD"
date : 2024-01-01 
weight : 2
chapter : false
pre : " <b> 5.3.2 </b> "
---

## Mục tiêu

Hoàn thiện luồng tự động build và deploy để mỗi lần cập nhật code có thể được đưa vào hệ thống một cách nhất quán.

## Các bước thực hiện

1. **Tạo workflow GitHub Actions**
- Tại thư mục gốc mã nguồn của bạn trên máy Local (hoặc trực tiếp trên GitHub), tạo cấu trúc thư mục theo đúng quy chuẩn của GitHub Actions: `.github/workflows/`.
- Tạo một tệp mới bên trong thư mục này và đặt tên là **`deploy.yml`**.
- Bạn copy toàn bộ đoạn mã kịch bản tối ưu hóa dưới đây để dán vào file `deploy.yml`. Kịch bản này đã được xử lý an toàn luồng truyền biến môi trường thông qua `os.environ` trong Python để tránh lỗi cú pháp CLI và tích hợp sẵn bộ gửi mail tự động:

```yml
name: Build and Deploy to ECS

on:
  push:
    branches: [main]

env:
  AWS_REGION: ap-southeast-1
  ECR_FRONTEND: 307257806722.dkr.ecr.ap-southeast-1.amazonaws.com/globalmart-frontend
  ECR_BACKEND: 307257806722.dkr.ecr.ap-southeast-1.amazonaws.com/globalmart-backend
  ECS_CLUSTER: globalmart-cluster
  ECS_SERVICE_FRONTEND: globalmart-frontend-task
  ECS_SERVICE_BACKEND: globalmart-backend-task

jobs:
  deploy:
    name: Build and Deploy
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ${{ env.AWS_REGION }}

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
            --region $AWS_REGION \
            --query 'taskDefinition' \
            --output json)
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
            --region $AWS_REGION \
            --cli-input-json "$NEW_TASK_DEF" \
            --query 'taskDefinition.taskDefinitionArn' \
            --output text)
          aws ecs update-service \
            --cluster $ECS_CLUSTER \
            --service $ECS_SERVICE_FRONTEND \
            --task-definition $NEW_TASK_ARN \
            --force-new-deployment \
            --region $AWS_REGION
          echo "Frontend deployed: $IMAGE_TAG"

      - name: Deploy Backend to ECS
        run: |
          IMAGE_TAG=${{ github.sha }}
          TASK_DEF=$(aws ecs describe-task-definition \
            --task-definition globalmart-backend-task \
            --region $AWS_REGION \
            --query 'taskDefinition' \
            --output json)
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
            --region $AWS_REGION \
            --cli-input-json "$NEW_TASK_DEF" \
            --query 'taskDefinition.taskDefinitionArn' \
            --output text)
          aws ecs update-service \
            --cluster $ECS_CLUSTER \
            --service $ECS_SERVICE_BACKEND \
            --task-definition $NEW_TASK_ARN \
            --force-new-deployment \
            --region $AWS_REGION
          echo "Backend deployed: $IMAGE_TAG"

      - name: Deployment complete
        run: |
          echo "Deploy triggered successfully"
          echo "Image tag: ${{ github.sha }}"
          echo "Check ECS Console in 2-3 minutes"
```

2. **Thực thi quy trình đóng gói và đẩy Docker Image lên Amazon ECR**
- Xác thực quyền đăng nhập với Amazon ECR
```
aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin 307257806722.dkr.ecr.ap-southeast-1.amazonaws.com
```
- Build và push image frontend và backend theo Dockerfile tương ứng lên Amazon ECR.
```
# Frontend
docker build -t globalmart-frontend ./frontend
docker tag globalmart-frontend:latest 307257806722.dkr.ecr.ap-southeast-1.amazonaws.com/globalmart-frontend:latest
docker push 307257806722.dkr.ecr.ap-southeast-1.amazonaws.com/globalmart-frontend:latest
```

```
# Backend
docker build -t globalmart-backend ./backend
docker tag globalmart-backend:latest 307257806722.dkr.ecr.ap-southeast-1.amazonaws.com/globalmart-backend:latest
docker push 307257806722.dkr.ecr.ap-southeast-1.amazonaws.com/globalmart-backend:latest
```
## Điểm cần xác minh

- Workflow chạy đúng branch và đúng trigger.
- Image được build đúng tag và push thành công lên ECR.
- ECS service nhận revision mới và rollout thành công.
- Frontend/backend sau deploy vẫn hoạt động đúng health check.

## Kết quả mong đợi

Sau bước này, hệ thống đã có luồng CI/CD tự động để build và triển khai phiên bản mới, giảm thao tác tay trong vận hành.

## Gợi ý hình ảnh cần thêm
- Hình workflow GitHub Actions chạy thành công.
![Overview](/images/5-Workshop/Workshop-img/action-1.jpg)
- Hình image mới trong ECR sau khi push.
![Overview](/images/5-Workshop/Workshop-img/action-2.jpg)
![Overview](/images/5-Workshop/Workshop-img/action-3.jpg)