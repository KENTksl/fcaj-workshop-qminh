---
title: "Create APIs"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.2.6.1. </b> "
---

## Objectives

Create the required API resources, routes, and integrations in API Gateway.

## Steps

1. Open **API Gateway** in the AWS Management Console.
2. Go to **APIs** → **Create API**.
3. Choose **HTTP API** → **Build**.

   ![API Setting 1](/images/5-Workshop/5.2-Prerequisite/5.2.6-APIGateway/api_setting1.png)

4. Configure the API:
   - API name: `globalmart-api`
   - IP address type: IPv4
   - Skip integrations for now

   ![API Setting 2](/images/5-Workshop/5.2-Prerequisite/5.2.6-APIGateway/api_setting2.png)

5. Create a route:
   - Go to **Routes** → **Create**
   - Route and method: `ANY /{proxy+}`

   ![API Setting 3](/images/5-Workshop/5.2-Prerequisite/5.2.6-APIGateway/api_setting3.png)

6. Create an integration (through VPC Link to internal ALB):
   - Go to **Integrations** → **Manage integrations** → **Create**
   - Integration type: **Private resource**
   - Selection method: **Select manually**
   - Target service: **ALB/NLB**
   - Load balancer: `alb-internal`
   - Listener: HTTP : 80
   - VPC link: `vpclink-globalmart` (created in 5.2.6.2)

   ![API Setting 4](/images/5-Workshop/5.2-Prerequisite/5.2.6-APIGateway/api_setting4.png)

7. Attach the integration to the route:
   - Go back to **Routes** → select `ANY /{proxy+}`
   - Select the integration and click **Attach integration**

   ![API Setting 5](/images/5-Workshop/5.2-Prerequisite/5.2.6-APIGateway/api_setting5.png)

## Expected results

- HTTP API `globalmart-api` has route `ANY /{proxy+}` and forwards traffic to `alb-internal` through the VPC Link.
