---
title: "Proposal"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Cloud Battleship Arena
## A Real-Time Multiplayer Battleship Game on AWS Serverless

### 1. Executive Summary
**Cloud Battleship Arena** is a modern, real-time multiplayer version of the classic Battleship game. The project uses a **Serverless architecture on Amazon Web Services (AWS)** to reduce server operations, scale with demand, and support real-time communication, combined with a responsive user interface built with **React 19 + Vite**.

The Backend uses managed AWS services and does not require a dedicated application server. Resources are billed by usage and can scale within the configured service limits.

---

### 2. Problem Statement

**The Problem**

Multiplayer applications need to maintain room state, process turns, and synchronize data between Clients. Operating a dedicated Backend over time can increase deployment, monitoring, and scaling work. For Battleship, the system must also respond quickly enough for both players to observe a consistent match state.

**The Solution**

Cloud Battleship Arena addresses these challenges with a **Serverless, event-driven architecture** on AWS:

- **Amazon Cognito** handles sign-up, sign-in, and user sessions; the JWT Authorizer currently protects user-profile endpoints.
- **Amazon API Gateway (HTTP & WebSocket APIs)** provides RESTful endpoints for room management/matchmaking and a persistent two-way WebSocket channel for real-time gameplay.
- **AWS Lambda (Node.js 24.x)** executes all backend logic: connection management, message routing, matchmaking, and match history.
- **Amazon DynamoDB** stores application data in the `Rooms`, `Connections`, `ChatMessages`, `User`, `EmailIndex`, and `MatchHistoryV2` tables.
- **Amazon S3** securely stores player avatars via Pre-signed URLs and hosts the static frontend.
- **Amazon CloudFront** distributes the static Frontend and Avatar content through a CDN. The Frontend calls the API Gateway HTTP and WebSocket endpoints directly.
- **AWS WAF** is attached to CloudFront to rate-limit abnormal requests to the Frontend layer; this Web ACL does not directly protect the current API Gateway endpoints.
- **AWS SAM (Serverless Application Model)** defines and deploys the Backend. Separate CloudFormation templates manage Frontend hosting and monitoring, while Cognito is configured manually.

**Expected Benefits**

- **Demo-friendly cost model**: Most Backend services use pay-per-use pricing; AWS WAF still has a base charge without traffic.
- **Demand-based scaling**: Lambda, API Gateway, and DynamoDB On-demand scale within account quotas and configured limits.
- **Real-world AWS experience**: The project covers architecture, Backend/Frontend development, deployment, monitoring, and an automated Frontend deployment pipeline.

---

### 3. Solution Architecture

The application uses a **Serverless, event-driven** architecture with three main layers:

1. **Frontend (SPA)**: React 19 + Vite, statically hosted on S3 and distributed via CloudFront, communicating with the Backend through HTTP API and WebSocket.
2. **Real-time Engine**: During gameplay, each player's client maintains a persistent **WebSocket API** connection to synchronize game state in real time (firing shots, placing ships, chat, game-over events).
3. **Serverless Backend**: **AWS Lambda** functions handle application logic and access **DynamoDB** to store state and enforce game rules, without operating a dedicated application server.

#### System Architecture Diagram

![Overall Cloud Battleship Arena AWS Architecture](/images/5-Workshop/5.2-Prerequisite/AWS_Architecture.png)

> **Diagram scope:** The CloudFront → API Gateway arrow represents the browser's next request after loading the SPA; CloudFront is not an API reverse proxy. The JWT label applies only to user-profile endpoints, while room APIs and WebSocket connections do not currently require JWT.

#### AWS Services Used

| Service | Role |
|---|---|
| **Amazon Cognito** | User authentication, JWT session token management |
| **Amazon API Gateway (HTTP)** | RESTful endpoints: create room, matchmaking, user profile |
| **Amazon API Gateway (WebSocket)** | Persistent two-way real-time channel during gameplay |
| **AWS Lambda (Node.js 24.x)** | Full game logic: connect/disconnect, fire, matchmaking, history |
| **Amazon DynamoDB** | Stores `Rooms`, `Connections`, `ChatMessages`, `User`, `EmailIndex`, and `MatchHistoryV2`; TTL applies to temporary room, connection, and chat data |
| **Amazon S3** | Avatar storage via Pre-signed URL; static frontend hosting |
| **Amazon CloudFront** | Frontend and Avatar delivery through OAC; it does not proxy HTTP/WebSocket APIs in the current architecture |
| **AWS WAF** | CloudFront-layer rate limiting; it does not directly protect API Gateway |
| **AWS SAM** | Infrastructure as Code for the serverless Backend |
| **GitHub Actions + OIDC** | Pipeline that builds and deploys the Frontend to S3/CloudFront |

#### Component Design

- **Frontend (React 19 + Vite)**: SPA with AWS Amplify for auth, Native WebSocket for real-time, Howler.js for audio, Phaser for animations, multilingual support (i18n), and responsive design.
- **WebSocket Handlers**: `wsConnect`, `wsDisconnect`, `wsMessage` — manage connections, validate turns, calculate hit/miss results on the Backend, and broadcast game state.
- **Room Management**: `createRoom`, `joinRoom`, `matchmakeRoom`, `getRoom`, `readyRoom`, `lobbyReadyRoom`, `rematchRoom`, `leaveRoom`.
- **User Management**: `createUser`, `getUser`, `updateUsername`, `getAvatarUploadUrl`, `getMatchHistory`.
- **Infrastructure Layer**: Separate CloudFormation stacks for frontend hosting (`frontend-hosting.yaml`) and CloudFront monitoring (`cloudfront-monitoring.yaml`).

---

### 4. Technical Implementation

#### Implementation Phases

The project follows the 9-week internship roadmap:

1. **LAB Training & Foundational Knowledge** *(Weeks 1–4)*:
   - **Week 1**: Get familiar with AWS and basic AWS services.
   - **Week 2**: Learn Amazon S3, CloudFront, and static Frontend delivery through a private bucket with OAC.
   - **Week 3**: Learn the fundamentals of IAM, Lambda, API Gateway, and DynamoDB.
   - **Week 4**: Define the requirements, scope, and initial architecture for Cloud Battleship Arena.

2. **Project Initiation & Core Backend Development** *(Weeks 5–6)*:
   - **Week 5**: Study IAM User/Policy/Role, discuss Battleship project specifications, and initialize Frontend.
   - **Week 6**: Integrate AWS Cognito for user authentication, design API Gateway & Lambda, set up DynamoDB storage, and plan PvP WebSockets.

3. **PvP, Game Logic & Advanced Features Development** *(Weeks 7–8)*:
   - **Week 7**: Complete real-time PvP gameplay via WebSockets, integrate S3 Pre-signed URLs for avatar uploads, finish user Profile profiles, and integrate sound effects.
   - **Week 8**: Complete matchmaking, chat, rematch, match history, leaderboard, and rank features.

4. **Optimization, Deployment & Documentation Handoff** *(Week 9)*:
   - **Week 9**: Enhance UI/UX, deploy the Frontend using S3 & CloudFront, resolve Lambda Cold Starts, and complete the AWS architecture diagrams.

#### Technical Requirements

- **Backend**: Node.js 24.x, AWS SAM CLI, AWS CLI.
- **Frontend**: Node.js v20+, React 19, Vite, AWS Amplify SDK, Native WebSocket, Howler.js, Phaser (animations).
- **Infrastructure**: AWS Account and a GitHub repository with Actions enabled. Cognito is created/linked manually; room APIs and WebSocket connections require additional JWT authorization before production use.

---

### 5. Timeline & Milestones

The project is structured across 4 phases aligned with the 9-week internship (details in Section 4). After go-live, the system is continuously monitored via **CloudWatch Dashboard** for key operational metrics:

| Phase | Weeks | Completion Milestone |
|---|---|---|
| Foundational Knowledge | 1–4 | Become familiar with AWS services directly relevant to the project and complete the initial design |
| Core Backend | 5–6 | Cognito, API Gateway, Lambda, DynamoDB operational |
| PvP & Game Logic | 7–8 | Real-time WebSocket, Avatar, matchmaking, match history, and rank |
| Deploy & Finalize | 9 | Frontend deployment pipeline, CloudFront, and architecture diagram |
| **Continuous Operations** | Post go-live | CloudWatch monitoring, Cost optimization |

---

### 6. Budget Estimation

The AWS Serverless **pay-per-use** model keeps costs low at low traffic, except for the AWS WAF base fee. The figures below estimate a very low-traffic demo environment; actual costs depend on Region, request volume, storage, and the Free Tier program applicable to the account:

| Service | Estimated/month |
|---|---|
| AWS Lambda | ~$0.00 (Free Tier: 1M requests/month) |
| Amazon API Gateway (HTTP) | ~$0.01 (2,000 requests) |
| Amazon API Gateway (WebSocket) | ~$0.02 (connection minutes) |
| Amazon DynamoDB (On-demand) | ~$0.00 (Free Tier: 25 GB) |
| Amazon S3 (Avatar + Frontend) | ~$0.05 |
| Amazon CloudFront | ~$0.01 (Free Tier: 1 TB/month) |
| Amazon Cognito | ~$0.00 (Free Tier: 50,000 MAU) |
| AWS WAF | ~$6.00 (Not covered by Free Tier: $5/Web ACL + $1/Rule) |
| **Total** | **~$6.10–$6.20/month** |

> *Note: AWS WAF has a base cost of approximately $6.00/month ($5 per Web ACL + $1 per Rule), excluding request-based charges. AWS Free Tier benefits vary by account creation date; check AWS Pricing and Billing before deployment.*

---

### 7. Risk Assessment

#### Risk Matrix

| Risk | Impact | Probability | Mitigation |
|---|---|---|---|
| High WebSocket latency | High | Low | Choose Region close to users; optimize Lambda cold start |
| DynamoDB throttling | Medium | Low | On-demand billing auto-scales; CloudWatch alarm |
| Cost overrun | Low | Low | Monitor AWS Billing, configure a Budget Alert for longer operation, and use TTL on supported temporary-data tables |
| Game state sync errors | High | Medium | Authoritative backend architecture; server-side validation |
| Unauthorized game API access | High | Medium | JWT protects user APIs; add authorization to room APIs and WebSocket |
| WebSocket disconnection | Medium | Medium | `wsDisconnect` cleans connection records; implement automatic Client reconnect/recovery |
| Missing automated tests | High | Medium | Add unit/integration tests and run lint/tests in the pipeline before production |

#### Contingency Plans

- If a WebSocket connection drops unexpectedly, the Backend cleans the connection record through `wsDisconnect`. The current Client does not automatically reconnect and re-link to the Room; this remains a planned improvement.
- Keep application code and IaC configuration in Git so a tested version can be redeployed when a deployment fails.
- CloudWatch Alarms send email notifications when Lambda errors or DynamoDB throttling exceeds thresholds.

---

### 8. Expected Outcomes

**Technical Achievements**
- Stable two-player real-time gameplay, with match state validated by the Backend and synchronized through WebSocket.
- Demand-based scaling through Lambda, API Gateway, and DynamoDB On-demand, within service quotas.
- Frontend pipeline: changes under `FrontEnd/` pushed to `main` or `develop` are built, deployed to S3, and followed by a CloudFront invalidation. The Backend remains deployed with the SAM CLI.

**Academic & Professional Value**
- Understand the fundamentals of Serverless event-driven architecture on AWS through a practical project.
- Hands-on experience with Design → Code → Deploy → Monitor; automated testing remains an area for further development.
- Gain hands-on experience with Cognito, API Gateway (HTTP + WebSocket), Lambda, DynamoDB, S3, CloudFront, SAM, and CloudWatch.
