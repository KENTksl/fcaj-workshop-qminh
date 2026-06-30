---
title: "Week 5 Worklog"
date: 2026-05-18
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---
### Week 5 Objectives:

* Complete Module 5 about AWS security, master the Shared Responsibility Model and core security services.
* Understand and practice IAM (User/Group/Policy/Role/Assume Role/Least Privilege), Amazon Cognito (User Pool/Identity Pool), AWS Organizations (OU/SCP), AWS Identity Center (SSO), AWS KMS, AWS Security Hub.
* Complete hands-on labs per Module 05-08 about AWS security.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 2   | - Study Module 5: AWS security theory and the Shared Responsibility Model <br>&emsp; + AWS security philosophy: Security of the Cloud vs Security in the Cloud <br>&emsp; + Shared Responsibility Model: AWS's responsibility (global infrastructure) and customer's responsibility (OS/firewall/IAM/encryption/application security) <br>&emsp; + Responsibility breakdown by service category (infrastructure/managed/fully managed) <br>&emsp; + Core security services: IAM, Cognito, Organizations & Identity Center, KMS <br>- Study IAM in detail: Root Account (best practice: don't use daily, enable MFA), IAM User/Group/Policy (JSON, Explicit Deny, AWS managed/Customer managed/Inline), IAM Role (Assume Role, least privilege), IAM for AWS services (no hard-code keys)                                                                                             | 18/05/2026 | 18/05/2026      | - |
| 3   | - Study Amazon Cognito (User Pool/Identity Pool, social login, fine-grained access control) <br>&emsp; + AWS Organizations (manage multiple accounts, OU, Consolidated Billing, SCP) <br>&emsp; + AWS Identity Center (SSO, Identity Source, Permission Sets, portal) <br>&emsp; + AWS KMS (Master Key/Data Key, encrypt/decrypt, FIPS 140-2 level 2) <br>&emsp; + AWS Security Hub (continuous security checks, AWS/PCI DSS standards, security score, Pass/Fail)                                                                                         | 19/05/2026 | 19/05/2026      | - |
| 4   | - **Module 05-08 Hands-on**: <br>&emsp; + **Lab44:** IAM Role & Condition (create User/Group as EC2/RDS admins, create Role with IP/time restrictions) <br>&emsp; + **Lab48:** Grant app access to AWS services with IAM Role (no hard-code access keys) <br>&emsp; + **Lab30:** Limit user permissions with IAM Permission Boundary (limit maximum permissions, reduce privilege escalation gaps)                                                                                         | 20/05/2026 | 20/05/2026      | - |
| 5   | - **Module 05-08 Hands-on (continued)**: <br>&emsp; + **Lab27:** Manage resources with Tag & Resource Groups (tag resources, use Resource Group to manage multiple resources at once) <br>&emsp; + **Lab28:** Manage EC2 access with Resource Tag via AWS IAM (apply least privilege with tag-based policy and role) <br>&emsp; + **Lab18:** Getting started with AWS Security Hub (aggregate security alerts, check compliance per AWS/PCI DSS standards)                                                                                         | 21/05/2026 | 21/05/2026      | - |
| 6   | - **Module 05-08 Hands-on (completed)**: <br>&emsp; + **Lab12:** Use AWS IAM Identity Center for strong identity management (set up SSO for multiple AWS accounts, assign permissions by group/team, least privilege) <br>&emsp; + **Lab33:** Introduction to AWS Key Management Service (KMS) (create Symmetric/Asymmetric CMK, set key policy, integrate with CloudTrail for logging) <br>&emsp; + **Lab22:** Optimize EC2 costs with Lambda (automatically start/stop instances with Lambda, learn about Saving Plan) <br>&emsp; + Recap all Module 5 content                                                                                         | 22/05/2026 | 22/05/2026      | - |


### Week 5 Achievements:

* Completed Module 5 and mastered all knowledge about AWS security:
  * Shared Responsibility Model: Security of the Cloud vs Security in the Cloud.
  * IAM: Root Account best practices, User/Group/Policy (JSON, Explicit Deny, 3 policy types), IAM Role (Assume Role, least privilege), IAM for AWS services.
  * Amazon Cognito: User Pool/Identity Pool, social login, fine-grained access control.
  * AWS Organizations: manage multiple accounts, OU, Consolidated Billing, SCP.
  * AWS Identity Center: SSO, Permission Sets, portal.
  * AWS KMS: Master Key/Data Key, encrypt/decrypt, FIPS 140-2 level 2.
  * AWS Security Hub: continuous security checks, AWS/PCI DSS standards.
* Understood and distinguished responsibility between AWS and customers by service type.
* Completed all hands-on labs per Module 05-08 about AWS security:
  * **Lab44**: IAM Role & Condition (create User/Group as EC2/RDS admins, create Role with IP/time restrictions).
  * **Lab48**: Grant app access to AWS services with IAM Role (no hard-code access keys).
  * **Lab30**: Limit user permissions with IAM Permission Boundary (limit maximum permissions, reduce privilege escalation gaps).
  * **Lab27**: Manage resources with Tag & Resource Groups (tag resources, use Resource Group to manage multiple resources at once).
  * **Lab28**: Manage EC2 access with Resource Tag via AWS IAM (apply least privilege with tag-based policy and role).
  * **Lab18**: Getting started with AWS Security Hub (aggregate security alerts, check compliance per AWS/PCI DSS standards).
  * **Lab12**: Use AWS IAM Identity Center for strong identity management (set up SSO for multiple AWS accounts, assign permissions by group/team, least privilege).
  * **Lab33**: Introduction to AWS Key Management Service (KMS) (create Symmetric/Asymmetric CMK, set key policy, integrate with CloudTrail for logging).
  * **Lab22**: Optimize EC2 costs with Lambda (automatically start/stop instances with Lambda, learn about Saving Plan).
