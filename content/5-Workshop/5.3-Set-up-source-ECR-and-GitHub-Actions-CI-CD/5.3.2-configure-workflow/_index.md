---
title : "Configure GitHub Actions and test CI/CD"
date : 2024-01-01 
weight : 2
chapter : false
pre : " <b> 5.3.2 </b> "
---

## Objectives

Complete the automated build and deploy flow so that each code update can be consistently deployed to the system.

## Implementation steps

1. **Create GitHub Actions workflow**
- At the root of your source code directory on Local machine (or directly on GitHub), create the directory structure following GitHub Actions standards: `.github/workflows/`.
- Create a new file inside this directory and name it **`deploy.yml`**.
- Copy the entire optimized script below to paste into the `deploy.yml` file. This script has been securely handling environment variable flow via `os.environ` in Python to avoid CLI syntax errors and has integrated automatic email sending:

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

2. **Execute packaging and pushing Docker Image to Amazon ECR**
- Authenticate login with Amazon ECR
```
aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin 307257806722.dkr.ecr.ap-southeast-1.amazonaws.com
```
- Build and push frontend and backend images according to respective Dockerfiles to Amazon ECR.
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

## Points to verify

- Workflow runs on correct branch and correct trigger.
- Image is built with correct tag and pushed successfully to ECR.
- ECS service receives new revision and rolls out successfully.
- Frontend/backend still passes health check after deployment.

## Expected results

After this step, the system has an automated CI/CD flow to build and deploy new versions, reducing manual operations in operations.

## Suggested images to add
- GitHub Actions workflow running successfully.
![Overview](/images/5-Workshop/Workshop-img/action-1.jpg)
- New image in ECR after push.
![Overview](/images/5-Workshop/Workshop-img/action-2.jpg)
![Overview](/images/5-Workshop/Workshop-img/action-3.jpg)
