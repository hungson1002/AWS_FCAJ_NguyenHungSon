---
title : "Introduction"
date : 2024-01-01 
weight : 1 
chapter : false
pre : " <b> 5.1. </b> "
---

#### Introduction to Serverless Event-Driven Architecture

**Serverless** does not mean "no servers" — it means you don't need to manage or maintain servers. AWS handles the infrastructure so you can focus on business logic.

The **Event-driven** architecture is the foundation of Cloud Battleship Arena:
+ When a player **connects** → API Gateway WebSocket triggers the `wsConnect` Lambda.
+ When a player **fires a shot** → `wsMessage` Lambda receives the coordinates, calculates the result, and broadcasts it to the opponent.
+ When a match **ends** → Lambda updates `MatchHistory` and ELO scores in DynamoDB.

#### Workshop Overview

In this workshop, you will build the complete backend for **Cloud Battleship Arena**, consisting of two main components:

+ **HTTP API** — Handles RESTful operations: create room (`createRoom`), join room (`joinRoom`), auto-matchmaking (`matchmakeRoom`), user profile management.
+ **WebSocket API** — Manages real-time connections: handshake (`wsConnect`), game action routing (`wsMessage`), cleanup on disconnect (`wsDisconnect`).

All infrastructure is defined as code (IaC) using **AWS SAM** (`template.yaml`) and can be deployed to AWS with a single command.

#### Architecture Overview

```
[React Frontend]
      |
      |-- HTTP Requests --> [API Gateway HTTP API] --> [Lambda Functions] --> [DynamoDB]
      |
      |-- WebSocket ------> [API Gateway WebSocket API] --> [Lambda wsConnect/wsMessage/wsDisconnect]
                                                                    |
                                                              [DynamoDB Connections/Rooms]
```

**AWS Services in this workshop:**

| Service | Role |
|---|---|
| **AWS SAM** | Define and deploy all infrastructure |
| **AWS Lambda** | Handle game logic and API processing |
| **Amazon API Gateway (HTTP)** | REST endpoints for room & user management |
| **Amazon API Gateway (WebSocket)** | Real-time channel for gameplay |
| **Amazon DynamoDB** | Store room state, connections, and match history |
| **Amazon Cognito** | Player authentication |
| **Amazon S3** | Store player avatars |