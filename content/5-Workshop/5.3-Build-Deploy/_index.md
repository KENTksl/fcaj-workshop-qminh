---
title: "Set up source code, IAM OIDC, and GitHub Actions CI/CD"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

## Objectives

This section focuses on building the automation flow from source code to container image and from container image to runtime environment. This is the first step for the project to have repeatable deployment capability and reduce manual operations.

## Main content

Section `5.3` is organized by the key steps in the CI/CD pipeline:

1. **Prepare IAM OIDC** for secure GitHub Actions authentication to AWS.
2. **Configure Workflow CI/CD** to automatically build, push images to ECR, and deploy to ECS.

## Child pages

1. [5.3.1 - Prepare IAM OIDC](5.3.1-set-up-iam-oidc/)
2. [5.3.2 - Configure Workflow CI/CD](5.3.2-set-up-github-action/)

## Expected results

After this section, you can push source code, build images, store images on ECR, and trigger pipelines to deploy new versions to the running environment.
