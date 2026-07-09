---
title : "Test HTTP API"
date : 2024-01-01 
weight : 2
chapter : false
pre : " <b> 5.3.2 </b> "
---

#### Get a JWT Token from Cognito for Testing

Before testing the API, you need a JWT token from Cognito. Register and sign in:

```bash
# Register a new user
aws cognito-idp sign-up \
  --client-id <CLIENT_ID> \
  --username testplayer@example.com \
  --password "Test@12345" \
  --region ap-southeast-1

# Confirm the account (skip email verification in test environment)
aws cognito-idp admin-confirm-sign-up \
  --user-pool-id <USER_POOL_ID> \
  --username testplayer@example.com \
  --region ap-southeast-1

# Sign in to get the token
aws cognito-idp initiate-auth \
  --client-id <CLIENT_ID> \
  --auth-flow USER_PASSWORD_AUTH \
  --auth-parameters USERNAME=testplayer@example.com,PASSWORD="Test@12345" \
  --region ap-southeast-1
```

Copy the `IdToken` value from the output — this is your JWT token.

#### Test Each HTTP API Endpoint

**1. Create User Profile (POST /api/users)**

```bash
curl -X POST https://<HttpApiUrl>/api/users \
  -H "Authorization: Bearer <JWT_TOKEN>" \
  -H "Content-Type: application/json" \
  -d '{"username": "Commander_Son"}'
```

Expected response:
```json
{"userId": "abc123", "username": "Commander_Son", "rankPoints": 0}
```

**2. Create a New Room (POST /api/rooms)**

```bash
curl -X POST https://<HttpApiUrl>/api/rooms \
  -H "Authorization: Bearer <JWT_TOKEN>" \
  -H "Content-Type: application/json"
```

Expected response:
```json
{"roomCode": "XK9P2M", "status": "waiting"}
```

**3. Join a Room (POST /api/rooms/{roomCode}/join)**

```bash
curl -X POST https://<HttpApiUrl>/api/rooms/XK9P2M/join \
  -H "Authorization: Bearer <JWT_TOKEN>"
```

**4. Auto Matchmaking (POST /api/matchmaking)**

```bash
curl -X POST https://<HttpApiUrl>/api/matchmaking \
  -H "Authorization: Bearer <JWT_TOKEN>"
```

Expected response (found an open room or created a new one):
```json
{"roomCode": "XK9P2M", "matched": true}
```

#### Verify in DynamoDB Console

After calling the API, open **AWS Console → DynamoDB → Tables → Rooms** and confirm the record was created with these fields:

- `roomCode` — Primary key (e.g., `XK9P2M`)
- `status` — Room status (`waiting` or `full`)
- `ttl` — Auto-delete time (Unix timestamp, typically 1 hour from now)

{{% notice note %}}
**TTL (Time To Live)** is a DynamoDB feature that automatically deletes expired items. In Cloud Battleship Arena, abandoned rooms are automatically deleted after 1 hour — no cleanup code needed.
{{% /notice %}}

#### Summary

Congratulations! You have successfully deployed the Backend with AWS SAM and tested the HTTP API endpoints. In the next section, we will connect via WebSocket to experience real-time gameplay.
