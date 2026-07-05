---
title: "Worklog Week 9"
date: 2026-06-15
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### Week 9 Objectives:

- Build the `GlobalMart` project with the frontend using `React` and the backend using `Java Spring Boot`.
- Complete core e-commerce system functionalities including registration, login, product management, category management, and an admin dashboard.
- Prepare the foundation for deploying `production CI/CD` for the project on AWS.

### Tasks to be Deployed This Week:

| Day | Tasks                                                                                                                                                                                                                                                                                                                                                                                                                           | Start Date | End Date   | Reference                                             |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | ---------- | ----------------------------------------------------- |
| 2   | - Initialize the `GlobalMart` project and organize the source code structure <br>&emsp; + Separate into 2 main parts: `globalmart-app` for frontend and `globalmart-api` for backend <br>&emsp; + Set up the development environment for `React/Vite` on the frontend and `Spring Boot` on the backend <br>&emsp; + Configure concurrent execution of frontend and backend from the root directory using `npm run dev`          | 15/06/2026 | 15/06/2026 | https://github.com/KENTksl/globalmart-production-cicd |
| 3   | - Develop the frontend using `React` and `TypeScript` <br>&emsp; + Build the basic user interface for the e-commerce website <br>&emsp; + Design pages for login, registration, and product display <br>&emsp; + Connect the frontend to the backend API to prepare for business logic processing                                                                                                                               | 16/06/2026 | 16/06/2026 | https://github.com/KENTksl/globalmart-production-cicd |
| 4   | - Develop the backend using `Java Spring Boot` <br>&emsp; + Build APIs for user login and registration <br>&emsp; + Build functionalities for product and category management <br>&emsp; + Connect the backend to `MongoDB Atlas` for application data storage                                                                                                                                                                  | 17/06/2026 | 17/06/2026 | https://github.com/KENTksl/globalmart-production-cicd |
| 5   | - Complete the main functionalities of the system <br>&emsp; + Integrate data workflow between frontend and backend <br>&emsp; + Add features including `login`, `register`, `product`, and `category` <br>&emsp; + Build and update the `admin dashboard` for system administration                                                                                                                                            | 18/06/2026 | 18/06/2026 | https://github.com/KENTksl/globalmart-production-cicd |
| 6   | - Prepare the `production CI/CD` foundation for the project <br>&emsp; + Add configuration files including `Dockerfile`, `buildspec.yml`, `appspec.yml`, and `taskdef.json` <br>&emsp; + Define integration directions for `AWS CodePipeline`, `CodeBuild`, `CodeDeploy`, `ECR/EC2`, `CloudWatch`, and `SNS` for the next deployment phase <br>&emsp; + Summarize project progress and review the overall source code structure | 19/06/2026 | 19/06/2026 | https://github.com/KENTksl/globalmart-production-cicd |

### Week 9 Achievements:

- Successfully built the `GlobalMart` project following a clear decoupled frontend and backend architecture:
  - Frontend utilizes `React`, `Vite`, and `TypeScript`.
  - Backend utilizes `Java Spring Boot`.
  - Source code is organized into 2 main directories: `globalmart-app` and `globalmart-api`.
- Completed core e-commerce system functionalities:
  - Account registration.
  - System login.
  - Product management.
  - Category management.
  - Added an `admin dashboard`.
- Connected the backend to `MongoDB Atlas` to store and manage application data.
- Configured an efficient workflow for running the project in the development environment:
  - Run frontend and backend concurrently using `npm run dev`.
  - Ability to run frontend or backend independently based on testing requirements.
- Prepared the foundation for deploying `production CI/CD` for the project:
  - Added necessary configuration files such as `Dockerfile`, `buildspec.yml`, `appspec.yml`, and `taskdef.json`.
  - Outlined the integration direction with AWS services including `CodePipeline`, `CodeBuild`, `CodeDeploy`, `CloudWatch`, and `SNS`.
