---
title : "Configure Workflow CI/CD"
date : 2026-06-30 
weight : 2
chapter : false
pre : " <b> 5.3.2. </b> "
---

## Objectives

Complete the configuration of the GitHub Actions workflow to automatically build, push Docker images to ECR, and deploy to ECS Fargate.

## Detailed implementation steps

### 1. Prepare GitHub Repository structure

Ensure your source code is organized clearly separated in the Repository so GitHub Actions can accurately locate the build position:

- **Frontend** application directory (contains `Dockerfile` for serving the interface).

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

- **Backend** application directory (contains `Dockerfile` for serving Spring Boot application).

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

### 2. Create GitHub Actions workflow

- At the root of your source code directory on Local machine (or directly on GitHub), create the directory structure following GitHub Actions standards: `.github/workflows/`.
- Create a new file inside this directory and name it **`deploy.yml`**.
- Copy the entire optimized script below to paste into the `deploy.yml` file.

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
            --message "Deploy successful!
          Repository: ${{ github.repository }}
          Branch: ${{ github.ref_name }}
          Commit: ${{ github.sha }}
          Message: ${{ github.event.head_commit.message }}
          Author: ${{ github.event.head_commit.author.name }}
          Time: $(date '+%Y-%m-%d %H:%M:%S UTC')
          See details: ${{ github.server_url }}/${{ github.repository }}/actions/runs/${{ github.run_id }}" \
            --region $AWS_REGION

      - name: Notify Failure via SNS
        if: failure()
        run: |
          aws sns publish \
            --topic-arn $SNS_TOPIC_ARN \
            --subject "GlobalMart Deploy FAILED" \
            --message "Deploy FAILED! Check immediately
          Repository: ${{ github.repository }}
          Branch: ${{ github.ref_name }}
          Commit: ${{ github.sha }}
          Message: ${{ github.event.head_commit.message }}
          Author: ${{ github.event.head_commit.author.name }}
          Time: $(date '+%Y-%m-%d %H:%M:%S UTC')
          See error: ${{ github.server_url }}/${{ github.repository }}/actions/runs/${{ github.run_id }}" \
            --region $AWS_REGION
```

---

## Expected results

After completing this lab, your GitHub Repository is fully prepared and ready to deploy CI/CD with GitHub Actions.

| Component | Status |
|---|---|
| Repository Structure | Frontend and Backend directories separated |
| Frontend Dockerfile | Configured to build React and run with Nginx |
| Backend Dockerfile | Configured to build and run Spring Boot application |
| `.github/workflows/` directory | Created |
| Workflow `deploy.yml` | Added to Repository |
| Workflow Trigger | Automatically runs on `push` to `main` branch |
| Permissions | `id-token: write` and `contents: read` declared |
| CI/CD Pipeline | Fully defined with Build, Push Image, Deploy ECS, and SNS notification steps |
