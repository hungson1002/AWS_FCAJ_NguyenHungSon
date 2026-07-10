---
title : "Deploy Backend with AWS SAM"
date : 2024-01-01 
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

### Goals

Successfully deploy the entire Cloud Battleship Arena Backend on AWS — including **8 Lambda functions**, **2 API Gateways** (HTTP & WebSocket), and **3 DynamoDB tables** — using the `template.yaml` configuration.

---

#### Step 1: Build the Backend Stack

```bash
cd BackEnd
sam build
```

---

#### Step 2: Deploy to AWS

```bash
sam deploy --guided
```

Configure parameters when prompted:

| Parameter | Value |
|---|---|
| Stack Name | `cloud-battleship-backend-dev` |
| AWS Region | `ap-southeast-1` |
| CognitoUserPoolId | `ap-southeast-1_VV7CeCaWL` |
| CognitoUserPoolClientId | `1vmsoep8dq5nlr1qvf8hpgsvs2` |
| CorsOrigin | `http://localhost:5173` |

After 3–5 minutes, the deployment completes successfully.

---

#### Step 3: Save Output Values

Copy the two URLs from the SAM **Outputs** section:

```
HttpApiUrl    → https://elh9fh33rd.execute-api.ap-southeast-1.amazonaws.com
WebSocketUrl  → wss://b9mxr6sqg6.execute-api.ap-southeast-1.amazonaws.com/prod
```

---

#### Verify Resources (AWS Console)

**CloudFormation Stack:**
1. Navigate to **AWS Console → CloudFormation → Stacks**.
2. Confirm the stack `cloud-battleship-backend-dev` shows a status of `CREATE_COMPLETE`.

![CloudFormation stack CREATE_COMPLETE](/images/5-Workshop/5.3-SAM-Backend/cloudformation-stack.png)

**DynamoDB Tables:**
1. Navigate to **AWS Console → DynamoDB → Tables**.
2. Verify that `Rooms`, `Connections`, and `User` tables are successfully provisioned.

![DynamoDB database tables](/images/5-Workshop/5.3-SAM-Backend/dynamodb-tables.png)

---

#### Content

- [Configure and Build SAM Stack](5.3.1-SAM-Build/)