---
title : "Prerequisites & Cognito Setup"
date : 2024-01-01 
weight : 2 
chapter : false
pre : " <b> 5.2. </b> "
---

### Goals

Install the required CLI tools, configure AWS CLI credentials, and provision the Amazon Cognito User Pool directly on the AWS Console.

---

#### Tool Requirements

| Tool | Version | Purpose |
|---|---|---|
| **Node.js** | v20+ | Runtime for Lambda functions and Frontend build |
| **AWS CLI** | v2 | Interact with AWS resources from the terminal |
| **AWS SAM CLI** | latest | Build & deploy the Serverless Backend stack |
| **Git** | any | Source code version control |

---

#### Step 1: Configure AWS CLI

```bash
aws configure
```

Enter your credentials when prompted:

```
AWS Access Key ID:     <access-key>
AWS Secret Access Key: <secret-key>
Default region:        ap-southeast-1
Output format:         json
```

Verify the connection is active:

```bash
aws sts get-caller-identity
```

---

#### Step 2: Clone the Project Repository

```bash
git clone https://github.com/HaoAboutMe/AWS_Cloud_Battleship_Arena.git
cd AWS_Cloud_Battleship_Arena
```

---

#### Step 3: Create Amazon Cognito User Pool (AWS Console)

1. Open **AWS Console → Cognito → User pools → Create user pool**.
2. **Configure sign-in experience**: Select **Email**. Click **Next**.
3. **Configure security requirements**: Keep defaults. Click **Next**.
4. **Configure sign-up experience**: Keep defaults. Click **Next**.
5. **Configure message delivery**: Select **Send email with Cognito**. Click **Next**.
6. **Integrate app**:
   - **User Pool Name**: `Battleship-Arena`.
   - **Initial app client**: Select **Public client**.
   - **App client name**: `CloudBattleshipArena`.
   - Uncheck **Generate client secret**.
7. Click **Next**, review all settings, then click **Create user pool**.

<img src="/images/5-Workshop/5.2-Prerequisite/cognito-user-pool.png" width="100%">

Navigate inside your newly created pool and save these two values:
- **User Pool ID** (formatted as `<YOUR_COGNITO_USER_POOL_ID>`)
- **Client ID** (formatted as `<YOUR_COGNITO_USER_POOL_CLIENT_ID>`)

<img src="/images/5-Workshop/5.2-Prerequisite/cognito-app-client.png" width="100%">