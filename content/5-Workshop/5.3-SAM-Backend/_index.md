---
title : "Deploy Backend with AWS SAM"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

#### Introduction to AWS SAM

**AWS SAM (Serverless Application Model)** is an IaC framework that lets you define an entire Serverless infrastructure in a single YAML file (`template.yaml`). SAM automatically creates AWS resources like Lambda, API Gateway, DynamoDB, IAM Roles, and more.

#### template.yaml Structure

The `BackEnd/template.yaml` file for Cloud Battleship Arena defines:

**DynamoDB Resources:**
- `RoomsTable` — Stores game room state with TTL for automatic cleanup of expired rooms.
- `ConnectionsTable` — Maps `connectionId` ↔ `roomCode` for WebSocket, with a `byRoomCode` GSI.
- `ChatMessagesTable` — Stores chat message history per room with TTL.

**API Gateway Resources:**
- `BattleshipHttpApi` — HTTP API with CORS and Cognito JWT Authorizer.
- `BattleshipWebSocketApi` — WebSocket API with route selection via `$request.body.action`.

**Lambda Resource example:**
```yaml
CreateRoomFunction:
  Type: AWS::Serverless::Function
  Properties:
    FunctionName: CreateRoom
    Handler: src/handlers/createRoom.handler
    Runtime: nodejs24.x
    Policies:
      - DynamoDBCrudPolicy:
          TableName: !Ref RoomsTable
    Events:
      CreateRoom:
        Type: HttpApi
        Properties:
          ApiId: !Ref BattleshipHttpApi
          Path: /api/rooms
          Method: POST
```

#### Step 1: Build the Backend

Navigate to the `BackEnd/` directory and run the build command:

```bash
cd BackEnd
sam build
```

SAM will install Node.js dependencies and package each Lambda function into a separate deployment artifact.

#### Step 2: First Deploy (Interactive Mode)

```bash
sam deploy --guided
```

SAM CLI will prompt for the following parameters:

```
Stack Name [sam-app]: cloud-battleship-backend
AWS Region [us-east-1]: ap-southeast-1
Parameter CognitoUserPoolId []: <USER_POOL_ID_from_step_5.2>
Parameter CognitoUserPoolClientId []: <CLIENT_ID_from_step_5.2>
Parameter CorsOrigin [http://localhost:5173]: http://localhost:5173
Confirm changes before deploy [y/N]: y
Allow SAM CLI IAM role creation [Y/n]: Y
Save arguments to configuration file [Y/n]: Y
```

After approximately 3–5 minutes, the stack will be created successfully.

#### Step 3: Retrieve Output Values

After deployment, the terminal displays the **Outputs** section. Note the following values:

```
Key                 HttpApiUrl
Value               https://xxxxxxxxxx.execute-api.ap-southeast-1.amazonaws.com

Key                 WebSocketUrl
Value               wss://yyyyyyyyyy.execute-api.ap-southeast-1.amazonaws.com/Prod
```

#### Step 4: Test the API

Try calling the HTTP API using `curl` or a tool like Postman:

```bash
# Create a new room (requires JWT token from Cognito)
curl -X POST https://<HttpApiUrl>/api/rooms \
  -H "Authorization: Bearer <JWT_TOKEN>" \
  -H "Content-Type: application/json"
```

Expected response:
```json
{
  "roomCode": "ABC123",
  "createdAt": "2026-07-09T06:00:00.000Z"
}
```

#### Content

- [Create and configure SAM stack](5.3.1-SAM-Build/)
- [Test the HTTP API](5.3.2-Test-HTTP-API/)