---
title : "Set up source, ECR, and GitHub Actions CI/CD"
date : 2024-01-01 
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

## Objectives

This section focuses on building the automation flow from source code to container image and from container image to runtime environment. This is the first step for the project to have repeatable deployment capability and reduce manual operations.

## Services used

- GitHub
- Amazon ECR
- Amazon S3
- GitHub Actions
- IAM

## Content

- [Prepare GitHub, IAM, ECR, and Artifact Bucket](5.3.1-create-gwe/)
- [Configure GitHub Actions and test CI/CD](5.3.2-test-gwe/)

## Expected results

After this section, you can push source code, build images, store images on ECR, and trigger pipelines to deploy new versions to the running environment.
