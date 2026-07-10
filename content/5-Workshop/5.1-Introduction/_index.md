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

#### Overall System Architecture Diagram

To get a high-level view of how the AWS services interact with each other in the Cloud Battleship Arena game, refer to the architecture diagram below:

![AWS Architecture Diagram](/images/5-Workshop/5.1-Introduction/AWS_Architecture.png)

This diagram describes the complete flow from when users access the game client via CloudFront, authenticate via Cognito, connect in real-time through WebSocket API Gateway, run serverless game logic on Lambda, and store persistent state in DynamoDB and S3.

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
| **Amazon CloudFront** | Single Entry Point, global content distribution, and API/WebSocket traffic proxy/security via WAF. |
| **GitHub Actions + OIDC** | Automates frontend build and deploy when code is pushed to the `main` branch. |

---

#### Hands-on Roadmap

1. **Prerequisites**: Configure AWS CLI credentials and create the Cognito User Pool.
2. **Backend**: Build & Deploy the Serverless API stack via AWS SAM. Test HTTP endpoints.
3. **Real-time**: Connect WebSocket clients, exchange payloads, and verify DynamoDB state.
4. **Frontend**: Deploy S3 Bucket + CloudFront Distribution via CloudFormation Console.
5. **CI/CD**: Configure IAM OIDC and GitHub Actions Workflow for full automation.
6. **Security & Cleanup**: Audit IAM Least Privilege policies and remove all resources.