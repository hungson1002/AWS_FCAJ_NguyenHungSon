---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Deploying Cloud Battleship Arena on AWS

#### Overview

In this workshop, we will walk through the entire hands-on process of packaging, configuring, and deploying the **Cloud Battleship Arena** multiplayer game on **Amazon Web Services (AWS)**.

This lab simulates 100% of the actual deployment steps you performed during the development of this product:
+ **Serverless Backend Deployment** using AWS SAM (Lambda, API Gateway HTTP/WebSocket, DynamoDB).
+ **Static Frontend Hosting** using Amazon S3 and global content delivery via CDN Amazon CloudFront.
+ **Automated CI/CD Pipelines** using GitHub Actions via secure IAM OIDC Federation (eliminating long-lived access keys).

#### Workshop Content

1. [Introduction](5.1-Introduction/)
2. [Prerequisites & Cognito Setup](5.2-Prerequisite/)
3. [Deploy Backend with AWS SAM](5.3-SAM-Backend/)
4. [Integrate Real-time WebSocket](5.4-WebSocket-Realtime/)
5. [Frontend Hosting (S3 & CloudFront)](5.5-Frontend-Hosting/)
6. [CI/CD Automation with GitHub Actions](5.6-CICD-Pipeline/)
7. [Integration Test](5.7-Integration-Test/)
8. [Clean Up Resources](5.8-Cleanup/)