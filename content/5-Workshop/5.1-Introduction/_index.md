---
title: "Introduction"
date: 2024-01-01 
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

### Goals

After completing this workshop, you will understand and deploy the end-to-end process of a real-time multiplayer Serverless application on AWS — covering Backend APIs, Frontend CDN distribution, fully automated CI/CD pipelines, and IAM/Cognito security hardening.

---

#### AWS Services Used in this Workshop

| Service | Role in the Project |
| :--- | :--- |
| **AWS SAM CLI** | Defines all Backend resources as IaC, packages code, and automates cloud deployment. |
| **AWS Lambda** | Runs serverless game logic (room creation, matchmaking, shot resolution, rank updates). |
| **Amazon API Gateway** | HTTP API handles REST requests. WebSocket API maintains persistent real-time connections. |
| **Amazon DynamoDB** | Stores game lobbies, WebSocket connections, match history, and player rank stats. |
| **Amazon Cognito** | Manages player registration, login, and provides JWT tokens to secure API calls. |
| **Amazon S3** | Hosts compiled static web assets (HTML, CSS, JS) for the React 19 frontend. |
| **Amazon CloudFront** | Global CDN for low-latency content delivery and secure S3 origin access via OAC. |
| **GitHub Actions + OIDC** | Automates frontend build and deploy when code is pushed to the `main` branch. |

---

#### Hands-on Roadmap

1. **Prerequisites**: Configure AWS CLI credentials and create the Cognito User Pool.
2. **Backend**: Build & Deploy the Serverless API stack via AWS SAM. Test HTTP endpoints.
3. **Real-time**: Connect WebSocket clients, exchange payloads, and verify DynamoDB state.
4. **Frontend**: Deploy S3 Bucket + CloudFront Distribution via CloudFormation Console.
5. **CI/CD**: Configure IAM OIDC and GitHub Actions Workflow for full automation.
6. **Security & Cleanup**: Audit IAM Least Privilege policies and remove all resources.