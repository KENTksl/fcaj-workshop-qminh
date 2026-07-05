---
title: "Set up ECR"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.2.3. </b> "
---

## Objectives

Prepare Amazon ECR to store Docker images for the frontend and backend.

## Steps

1. Open the **ECR Dashboard** in the AWS Management Console.
2. Go to **Private registry** → **Repositories** → **Create repository**.

3. Create repository `globalmart-frontend`:
   - Repository name: `globalmart-frontend`
   - Image tag mutability: **Mutable**
   - Encryption: **AES-256**

   ![ECR Setting 1](/images/5-Workshop/5.2-Prerequisite/5.2.3-ECR/ecr_setting1.png)

4. Create repository `globalmart-backend` with the same settings:

   ![ECR Setting 2](/images/5-Workshop/5.2-Prerequisite/5.2.3-ECR/ecr_setting2.png)

## Expected results

- You have 2 repositories `globalmart-frontend` and `globalmart-backend` ready for pushing images and creating ECS task definitions.

  ![ECR Completed](/images/5-Workshop/5.2-Prerequisite/5.2.3-ECR/ecr_complete.png)
