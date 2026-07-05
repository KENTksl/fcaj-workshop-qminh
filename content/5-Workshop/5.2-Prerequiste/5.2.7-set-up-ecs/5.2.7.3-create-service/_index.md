---
title: "Create Service"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.2.7.3. </b> "
---

## Objectives

Create ECS services to deploy the frontend and backend on the prepared cluster.

## Steps

1. Open **Amazon Elastic Container Service (ECS)**.
2. Go to **Clusters** → select cluster `globalmart`.
3. In the **Services** tab, click **Create**.

**Frontend service**

4. In **Service details**:
   - Task definition family: `globalmart-frontend-task`
   - Task definition revision: Latest
   - Service name: `globalmart-frontend-service`

   ![Service frontend setting 1](/images/5-Workshop/5.2-Prerequisite/5.2.7-ECS/service_setting1.png)
   ![Service frontend setting 2](/images/5-Workshop/5.2-Prerequisite/5.2.7-ECS/service_setting2.png)

5. In **Load balancing**:
   - Use load balancing: enabled
   - Load balancer type: Application Load Balancer
   - Use an existing load balancer: `alb-internet-facing`
   - Listener: HTTP : 80
   - Target group: `tg-frontend`
   - Health check path: `/`

   ![Service frontend setting 4](/images/5-Workshop/5.2-Prerequisite/5.2.7-ECS/service_setting4.png)
   ![Service frontend setting 5](/images/5-Workshop/5.2-Prerequisite/5.2.7-ECS/service_setting5.png)

**Backend service**

6. Create another service for backend:
   - Task definition family: `globalmart-backend-task`
   - Task definition revision: Latest
   - Service name: `globalmart-backend-service`

   ![Service backend setting 6](/images/5-Workshop/5.2-Prerequisite/5.2.7-ECS/service_setting6.png)

7. In **Networking**:
   - VPC: `globalmart-vpc`
   - Subnets: select private subnets
   - Security group: `ecs-backend-sg`
   - Public IP: Turned off

   ![Service backend setting 7](/images/5-Workshop/5.2-Prerequisite/5.2.7-ECS/service_setting7.png)

8. In **Load balancing**:
   - Use an existing load balancer: `alb-internal`
   - Listener: HTTP : 80
   - Target group: `tg-backend`
   - Health check path: `/actuator/health`

   ![Service backend setting 8](/images/5-Workshop/5.2-Prerequisite/5.2.7-ECS/service_setting8.png)
   ![Service backend setting 9](/images/5-Workshop/5.2-Prerequisite/5.2.7-ECS/service_setting9.png)

## Expected results

- In cluster `globalmart`, you have 2 services `globalmart-frontend-service` and `globalmart-backend-service` running (Active/Healthy).

  ![Service completed](/images/5-Workshop/5.2-Prerequisite/5.2.7-ECS/service_complete.png)
