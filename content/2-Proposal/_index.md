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
**Cloud Battleship Arena** is a modern, real-time multiplayer version of the classic Battleship game. The project leverages a fully **Serverless architecture on Amazon Web Services (AWS)** to deliver extreme scalability and ultra-low latency, combined with a vibrant, responsive user interface built with **React 19 + Vite**.

The entire system requires no physical server management — AWS automatically scales with player load, optimizing cost and reliability for a real-time gaming experience.

---

### 2. Problem Statement

**The Problem**

Traditional multiplayer games typically require dedicated physical servers, leading to high operational costs, difficulty scaling to match player demand, and complex maintenance overhead. For real-time games like Battleship, network latency is critical — every millisecond affects the player experience.

**The Solution**

Cloud Battleship Arena addresses these challenges with a fully **Serverless, event-driven architecture** on AWS:

- **Amazon Cognito** handles secure user authentication.
- **Amazon API Gateway (HTTP & WebSocket APIs)** provides RESTful endpoints for room management/matchmaking and a persistent two-way WebSocket channel for real-time gameplay.
- **AWS Lambda (Node.js 24.x)** executes all backend logic: connection management, message routing, matchmaking, and match history.
- **Amazon DynamoDB** provides high-speed, ultra-low-latency storage for `Rooms`, `Connections`, `ChatMessages`, `User`, and `MatchHistory` tables.
- **Amazon S3** securely stores player avatars via Pre-signed URLs and hosts the static frontend.
- **Amazon CloudFront** acts as a Single Entry Point, distributing the static frontend globally with low latency and securing the entire application (HTTP & WebSocket APIs) via AWS WAF.
- **AWS SAM (Serverless Application Model)** defines, packages, and deploys the entire infrastructure as code (IaC).

**Benefits and Return on Investment**

- **Near-zero cost when idle**: AWS pay-per-use only charges for actual requests.
- **Automatic scaling**: No concerns about traffic spikes — the system scales seamlessly.
- **Real-world AWS experience**: The project covers the full software development lifecycle — from architecture design and Backend/Frontend development to automated CI/CD with GitHub Actions.

---

### 3. Solution Architecture

The application follows a fully **Serverless, event-driven** architecture with three main layers:

1. **Frontend (SPA)**: React 19 + Vite, statically hosted on S3 and distributed via CloudFront, communicating with the Backend through HTTP API and WebSocket.
2. **Real-time Engine**: During gameplay, each player's client maintains a persistent **WebSocket API** connection to synchronize game state in real time (firing shots, placing ships, chat, game-over events).
3. **Serverless Backend**: **AWS Lambda** functions handle all computation, accessing **DynamoDB** securely to store state and enforce game logic — with zero server management.

#### System Architecture Diagram

![Overall Cloud Battleship Arena AWS Architecture](/images/5-Workshop/5.2-Prerequisite/AWS_Architecture.png)

#### AWS Services Used

| Service | Role |
|---|---|
| **Amazon Cognito** | User authentication, JWT session token management |
| **Amazon API Gateway (HTTP)** | RESTful endpoints: create room, matchmaking, user profile |
| **Amazon API Gateway (WebSocket)** | Persistent two-way real-time channel during gameplay |
| **AWS Lambda (Node.js 24.x)** | Full game logic: connect/disconnect, fire, matchmaking, history |
| **Amazon DynamoDB** | Stores `Rooms`, `Connections`, `ChatMessages`, `User`, `MatchHistory` with automatic TTL |
| **Amazon S3** | Avatar storage via Pre-signed URL; static frontend hosting |
| **Amazon CloudFront** | Single Entry Point, global frontend distribution, API and WebSocket traffic proxy/security via WAF |
| **AWS SAM** | Infrastructure as Code — defines and deploys all backend resources |
| **GitHub Actions + OIDC** | Automated CI/CD: build and deploy Frontend to S3/CloudFront |

#### Component Design

- **Frontend (React 19 + Vite)**: SPA with AWS Amplify for auth, Native WebSocket for real-time, Howler.js for audio, Phaser for animations, multilingual support (i18n), and responsive design.
- **WebSocket Handlers**: `wsConnect`, `wsDisconnect`, `wsMessage` — manage connection lifecycle and broadcast game state to opponents.
- **Room Management**: `createRoom`, `joinRoom`, `matchmakeRoom`, `getRoom`, `readyRoom`, `lobbyReadyRoom`, `rematchRoom`, `leaveRoom`.
- **User Management**: `createUser`, `getUser`, `updateUsername`, `getAvatarUploadUrl`, `getMatchHistory`.
- **Infrastructure Layer**: Separate CloudFormation stacks for frontend hosting (`frontend-hosting.yaml`) and CloudFront monitoring (`cloudfront-monitoring.yaml`).

---

### 4. Technical Implementation

#### Implementation Phases

The project is structured scientifically to align with the actual 9-week internship roadmap:

1. **LAB Training & Foundational Knowledge** *(Weeks 1–4)*:
   - **Week 1**: Get familiar with AWS and basic AWS services.
   - **Week 2**: Study Amazon S3, CloudFront, and Static Website Hosting.
   - **Week 3**: Study AWS Backup and Virtual Machine migration to AWS.
   - **Week 4**: Study AWS Networking, EC2, and plan the internship project.

2. **Project Initiation & Core Backend Development** *(Weeks 5–6)*:
   - **Week 5**: Study IAM User/Policy/Role, discuss Battleship project specifications, and initialize Frontend.
   - **Week 6**: Integrate AWS Cognito for user authentication, design API Gateway & Lambda, set up DynamoDB storage, and plan PvP WebSockets.

3. **PvP, Game Logic & Advanced Features Development** *(Weeks 7–8)*:
   - **Week 7**: Complete real-time PvP gameplay via WebSockets, integrate S3 Pre-signed URLs for avatar uploads, finish user Profile profiles, and integrate sound effects.
   - **Week 8**: Develop other functions and configure API Gateway Throttling.

4. **Optimization, Deployment & Documentation Handoff** *(Week 9)*:
   - **Week 9**: Enhance UI/UX, deploy the Frontend using S3 & CloudFront, resolve Lambda Cold Starts, and complete the AWS architecture diagrams.

#### Technical Requirements

- **Backend**: Node.js 24.x, AWS SAM CLI, AWS CLI.
- **Frontend**: Node.js v20+, React 19, Vite, AWS Amplify SDK, Native WebSocket, Howler.js, Phaser (animations).
- **Infrastructure**: AWS Account, GitHub repository with Actions enabled.

---

### 5. Timeline & Milestones

Refer to the four implementation phases detailed in **Section 4 — Technical Implementation** above. Beyond the four main development phases, the system runs continuously post go-live with **CloudWatch Dashboard** monitoring for Lambda errors, DynamoDB throttling, and cost optimization based on real-world traffic.

---

### 6. Budget Estimation

The **pay-per-use** AWS Serverless model ensures extremely low costs at low traffic, with the exception of the fixed base cost for AWS WAF:

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

> *Note: AWS WAF has a fixed base cost of approximately $6.00/month ($5 per Web ACL + $1 per Rule). In testing or workshop environments, it is recommended for students to delete the Web ACL after completing the lab to avoid ongoing charges.*

---

### 7. Risk Assessment

#### Risk Matrix

| Risk | Impact | Probability | Mitigation |
|---|---|---|---|
| High WebSocket latency | High | Low | Choose Region close to users; optimize Lambda cold start |
| DynamoDB throttling | Medium | Low | On-demand billing auto-scales; CloudWatch alarm |
| Cost overrun | Low | Low | AWS Budget Alert; TTL auto-deletes stale data |
| Game state sync errors | High | Medium | Authoritative backend architecture; server-side validation |
| WebSocket disconnection | Medium | Medium | Handle `wsDisconnect` gracefully; client-side reconnect logic |

#### Contingency Plans

- If a WebSocket connection drops: Client-side session reconnection logic will restore the gameplay state using the previous Connection ID without interrupting the match.
- Use CloudFormation to quickly roll back to the last stable version.
- CloudWatch Alarms send email notifications when Lambda errors or DynamoDB throttling exceeds thresholds.

---

### 8. Expected Outcomes

**Technical Achievements**
- Smooth real-time gameplay with WebSocket latency under 100ms.
- Automatic scaling in response to real-world demand thanks to the Serverless architecture — no manual intervention required during traffic spikes.
- Complete CI/CD pipeline: every push to `main` automatically deploys to Production.

**Academic & Professional Value**
- Deep understanding of Serverless event-driven architecture on AWS.
- Hands-on experience with the full software development lifecycle: Design → Code → Test → Deploy → Monitor.
- Proficiency in key AWS services: Cognito, API Gateway (HTTP + WebSocket), Lambda, DynamoDB, S3, CloudFront, SAM, CloudWatch.