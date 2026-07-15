---
title: "Practical Results"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

## Objectives

Summarize the practical results after completing the workshop, including evidence that the CI/CD pipeline, application deployment, and monitoring system are functioning correctly on AWS.

## Main Content

Section `5.6` presents the practical results achieved during the workshop:

### 1. Developer Pushes Code to GitHub and Triggers the CI/CD Pipeline

The developer commits and pushes the latest source code to the GitHub repository. GitHub Actions is automatically triggered to build, package, and deploy the application.

![Overview](/images/5-Workshop/Workshop-img/result-1.jpg)

![Overview](/images/5-Workshop/Workshop-img/result-2.jpg)

---

### 2. Deployment Notification via Gmail

After the deployment process is completed, Amazon SNS automatically sends an email notification indicating whether the deployment succeeded or failed.

![Overview](/images/5-Workshop/Workshop-img/result-6.jpg)

---

### 3. Deployment Results on Amazon ECS

Both ECS services are successfully deployed and running:

- **globalmart-frontend-service** is in the **Active** state with **1/1 running task**, using the latest Task Definition revision.
- **globalmart-backend-service** is in the **Active** state with **1/1 running task**, using the latest Task Definition revision.

![Overview](/images/5-Workshop/Workshop-img/result-3.jpg)

![Overview](/images/5-Workshop/Workshop-img/result-4.jpg)

---

### 4. Updated Application After Deployment

After the deployment is completed, the website is successfully updated with the latest source code. In this example, the newly deployed **Profile Update** feature is available and functioning correctly.

![Overview](/images/5-Workshop/Workshop-img/result-5.jpg)

---

### 5. CloudWatch High CPU Alert Notification

When the backend CPU utilization exceeds the configured threshold, Amazon CloudWatch triggers an alarm and Amazon SNS automatically sends an email notification to the subscribed administrator.

![Overview](/images/5-Workshop/Workshop-img/result-7.jpg)

---

## Live Demo Video
**Demo Video:** [Watch the demo video](https://drive.google.com/file/d/1oNYPaeILVUyJaDqz2qYWPu9UwtY6KnCG/view)

## Project Results
1. **Project Source Code - GitHub:** [https://github.com/KENTksl/globalmart-production-cicd](https://github.com/KENTksl/globalmart-production-cicd)
2. **Website:** [http://alb-internet-facing-1015453304.ap-southeast-1.elb.amazonaws.com/](http://alb-internet-facing-1015453304.ap-southeast-1.elb.amazonaws.com/)
