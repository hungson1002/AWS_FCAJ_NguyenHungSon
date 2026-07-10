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

#### Resource Structure in template.yaml

The `BackEnd/template.yaml` file defines the complete Backend including:
- **Globals**: Runtime `nodejs24.x`, 10s timeout, environment variables shared across all 8 Lambdas.
- **BattleshipHttpApi**: HTTP API Gateway with a JWT Authorizer tied to Cognito.
- **BattleshipWebSocketApi**: WebSocket API Gateway with route selection based on the `action` field.
- **RoomsTable / ConnectionsTable / ChatMessagesTable**: 3 DynamoDB tables with TTL and GSI.

---

#### Step 1: Install Dependencies

```bash
cd AWS_Cloud_Battleship_Arena/BackEnd
npm install
```

---

#### Step 2: Build SAM Artifact

```bash
sam build
```

SAM reads `template.yaml`, runs `npm install` for each Lambda, and packages them into `.aws-sam/build/`.

---

#### Step 3: Validate Template (Optional)

```bash
sam validate --lint
```

---

#### Step 4: Deploy to AWS

```bash
sam deploy --guided
```

Configure parameters when prompted:

| Parameter | Value |
|---|---|
| Stack Name | `cloud-battleship-backend-dev` |
| AWS Region | `ap-southeast-1` |
| CognitoUserPoolId | `<YOUR_COGNITO_USER_POOL_ID>` |
| CognitoUserPoolClientId | `<YOUR_COGNITO_USER_POOL_CLIENT_ID>` |
| CorsOrigin | `http://localhost:5173` |

After 3–5 minutes, the deployment completes successfully. Save the two URLs from the SAM **Outputs** section:

```
HttpApiUrl    → https://elh9fh33rd.execute-api.ap-southeast-1.amazonaws.com
WebSocketUrl  → wss://b9mxr6sqg6.execute-api.ap-southeast-1.amazonaws.com/prod
```

{{% notice tip %}}
After running `sam deploy --guided` for the first time, SAM saves all parameters to `samconfig.toml`. Subsequent deployments only need `sam deploy` — no `--guided` flag required.
{{% /notice %}}

---

#### Step 5: Configure Cognito Post-Confirmation Trigger

To automatically persist registered user profiles to our DynamoDB table after they verify their account, we need to bind the Cognito SignUp event to our `CreateUser` Lambda function:

1. Navigate to the **Amazon Cognito Console** > Select your `Battleship-Arena` User Pool.
2. Navigate to the **User pool properties** tab.
3. Scroll down to the **Lambda triggers** section and click **Add Lambda trigger**.
4. Configure trigger details:
   - **Trigger type**: Select **Sign-up** > Check **Post confirmation trigger** (Triggers after signup verification succeeds).
   - **Assign Lambda function**: Select **Choose an existing Lambda function**.
   - **Lambda function**: Select our project's `CreateUser` function.
5. Click **Add Lambda trigger** to save the binding.

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

**API Gateway Routes:**
1. Navigate to **AWS Console → API Gateway → APIs**.
2. Select the HTTP API of the project (e.g., `cloud-battleship-backend-dev`).
3. Click on **Routes** in the left menu. Verify that all HTTP API endpoints are correctly displayed under the `/api` route.

![API Gateway Routes Structure](/images/5-Workshop/5.3-SAM-Backend/AllAPI.jpg)

**Lambda Functions:**
1. Navigate to **AWS Console → Lambda → Functions**.
2. Verify that all project Lambda functions have been created and are configured to use the `Node.js 24.x` runtime.

![Lambda Functions List](/images/5-Workshop/5.3-SAM-Backend/AllLambda.jpg)
