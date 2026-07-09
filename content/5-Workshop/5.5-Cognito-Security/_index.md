---
title : "Secure APIs with Cognito (Advanced)"
date : 2024-01-01
weight : 5
chapter : false
pre : " <b> 5.5. </b> "
---

When you create an HTTP API with API Gateway, you can attach an **Authorizer** to control access. The **Amazon Cognito JWT Authorizer** integrates directly with API Gateway, validating player tokens without writing any additional code.

You can also create detailed **IAM Policies** to restrict Lambda permissions according to the **Least Privilege** principle — each function only has access to the DynamoDB tables it actually needs.

In this section, you will verify the security flow of Cloud Battleship Arena and configure a more detailed Lambda policy.

#### Step 1: Verify the Cognito Authorizer is Working

Try calling a protected API **without** a token:

```bash
curl -X GET https://<HttpApiUrl>/api/users/me
```

Expected response (401 Unauthorized):
```json
{"message": "Unauthorized"}
```

Call again **with** a valid JWT token:
```bash
curl -X GET https://<HttpApiUrl>/api/users/me \
  -H "Authorization: Bearer <JWT_TOKEN>"
```

Expected response (200 OK):
```json
{
  "userId": "abc123",
  "username": "Commander_Son",
  "rankPoints": 1200
}
```

#### Step 2: Inspect Lambda Least Privilege Policy

In `template.yaml`, each Lambda function is attached a policy following the least privilege principle:

```yaml
# GetUserFunction only has READ permission on UserTable
GetUserFunction:
  Type: AWS::Serverless::Function
  Properties:
    Policies:
      - DynamoDBReadPolicy:
          TableName: !Ref UserTable

# CreateRoomFunction only has CRUD permission on RoomsTable
CreateRoomFunction:
  Type: AWS::Serverless::Function
  Properties:
    Policies:
      - DynamoDBCrudPolicy:
          TableName: !Ref RoomsTable
```

Open **AWS Console → IAM → Roles**, find the `GetUser` Lambda role. Confirm it only has `dynamodb:GetItem`, `dynamodb:Query`, `dynamodb:Scan` — no write permissions.

#### Step 3: Create a Custom Policy (Advanced)

You want to restrict the `wsMessage` Lambda to only broadcast to connectionIds within the same room. Create a resource-based DynamoDB policy:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowRoomScopedRead",
      "Effect": "Allow",
      "Action": [
        "dynamodb:Query",
        "dynamodb:GetItem"
      ],
      "Resource": [
        "arn:aws:dynamodb:*:*:table/Connections",
        "arn:aws:dynamodb:*:*:table/Connections/index/byRoomCode"
      ]
    }
  ]
}
```

#### Step 4: Verify the Policy Works Correctly

Try connecting via WebSocket with a token from a different room and confirm the Lambda rejects the broadcast:

```bash
# Connect with wrong roomCode
wscat -c "wss://<WebSocketUrl>?roomCode=WRONG_ROOM"
# Send FIRE action → Server returns 403 error
{"action": "FIRE", "row": 0, "col": 0}
```

Expected response:
```json
{"error": "Forbidden: You are not in this room"}
```

In this section, you have:
- Verified that Cognito JWT Authorizer protects the HTTP API.
- Inspected the Least Privilege principle in Lambda IAM Roles.
- Created a custom policy to control the scope of DynamoDB access.
