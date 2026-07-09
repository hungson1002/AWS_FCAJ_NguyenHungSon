---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Building a Serverless Backend for Cloud Battleship Arena

#### Overview

**AWS Serverless** allows you to run applications without managing servers — AWS automatically scales on demand and you only pay for what you actually use.

In this workshop, we will learn how to design, deploy, and test a complete backend system for a real-time multiplayer battleship game (**Cloud Battleship Arena**) using **AWS SAM**, **AWS Lambda**, **Amazon API Gateway** (HTTP & WebSocket), **Amazon DynamoDB**, and **Amazon Cognito**.

We will build two types of APIs:
+ **HTTP API (REST)** — For managing game rooms, user profiles, and matchmaking.
+ **WebSocket API** — For transmitting real-time game state between players.

#### Content

1. [Workshop Overview](5.1-Workshop-overview/)
2. [Environment Setup](5.2-Prerequiste/)
3. [Deploy Backend with AWS SAM](5.3-SAM-Backend/)
4. [Integrate Real-time WebSocket](5.4-WebSocket-Realtime/)
5. [Secure APIs with Cognito (Advanced)](5.5-Cognito-Security/)
6. [Clean Up Resources](5.6-Cleanup/)