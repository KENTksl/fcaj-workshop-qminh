---
title: "Create Task Definitions"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.2.7.1. </b> "
---

## Objectives

Create task definitions for the frontend and backend so ECS can run containers with the correct configuration.

## Steps

1. Open **Amazon Elastic Container Service (ECS)**.
2. Go to **Task definitions** → **Create new task definition**.

3. Create the **Frontend task definition**:
   - Task definition family: `globalmart-frontend-task`
   - Launch type: **AWS Fargate**
   - OS/Architecture: Linux/x86_64
   - Task size: CPU **1 vCPU**, Memory **3 GB**

   ![Task definitions 1](/images/5-Workshop/5.2-Prerequisite/5.2.7-ECS/task_setting1.png)

4. Configure container `frontend`:
   - Container name: `frontend`
   - Image URI: select from ECR repository `globalmart-frontend`
   - Port mappings: container port **80** (TCP), app protocol **HTTP**

   ![Task definitions 2](/images/5-Workshop/5.2-Prerequisite/5.2.7-ECS/task_setting2.png)

   Select the image from ECR (if the repo has no images yet, it will show “No images”).

   ![Task definitions 3](/images/5-Workshop/5.2-Prerequisite/5.2.7-ECS/task_setting3.png)

5. Create the **Backend task definition** with similar settings:
   - Task definition family: `globalmart-backend-task`
   - Container name: `backend`
   - Image URI: select from ECR repository `globalmart-backend`
   - Container port: **8080**
   - Environment variables (do not store the password in this document):
     - `SPRING_DATASOURCE_URL`
     - `SPRING_DATASOURCE_USERNAME`
     - `SPRING_DATASOURCE_PASSWORD`

## Expected results

- You have 2 task definitions `globalmart-frontend-task` and `globalmart-backend-task` (Active).

  ![Task complete](/images/5-Workshop/5.2-Prerequisite/5.2.7-ECS/task_complete.png)
