---
title : "Prerequisites & Cognito"
date : 2026-07-10 
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---

### Goals

Establish all foundational requirements before deploying the system — including creating an IAM User, configuring AWS CLI with Access Keys, and provisioning an **Amazon Cognito User Pool** for user authentication in **Cloud Battleship Arena**.

---

#### Why complete this section first?

1. **AWS Access**: An IAM User and Access Key are prerequisites for AWS CLI and SAM CLI to interact with your AWS account.
2. **User Authentication**: The Cognito User Pool must be created in advance because the **User Pool ID** and **Client ID** will be passed as parameters during the SAM Backend deployment in step 5.3.
3. **Project Source Code**: Clone the repository locally to prepare for the build and deploy steps that follow.

---

#### Content

- [Environment Setup](5.2.1-Environment-Setup/)
- [Amazon Cognito Setup](5.2.2-Cognito-Setup/)