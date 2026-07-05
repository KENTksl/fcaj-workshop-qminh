---
title: "Create Cluster"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.2.7.2. </b> "
---

## Objectives

Create the ECS cluster that will run the GlobalMart services.

## Steps

1. Open **Amazon Elastic Container Service (ECS)**.
2. Go to **Clusters** → **Create cluster**.
3. Configure:
   - Cluster name: `globalmart`
   - Infrastructure: **Fargate only**

   ![Cluster setting](/images/5-Workshop/5.2-Prerequisite/5.2.7-ECS/cluster_setting1.png)
4. Click **Create**.

## Expected results

- ECS cluster `globalmart` is created successfully and ready for creating services.
