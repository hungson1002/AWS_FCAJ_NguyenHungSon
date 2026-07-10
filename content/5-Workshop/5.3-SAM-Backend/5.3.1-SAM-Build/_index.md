---
title : "Configure and Build SAM Stack"
date : 2024-01-01 
weight : 1
chapter : false
pre : " <b> 5.3.1 </b> "
---

### Goals

Understand the `template.yaml` structure and successfully package the entire Backend into a deployment artifact ready for AWS.

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

#### Verify Build Output

```bash
ls .aws-sam/build/
# Expected: CreateRoomFunction/ JoinRoomFunction/ WebSocketConnectFunction/ ...
```

Each directory corresponds to one independently packaged Lambda function.

{{% notice tip %}}
After running `sam deploy --guided` for the first time, SAM saves all parameters to `samconfig.toml`. Subsequent deployments only need `sam deploy` — no `--guided` flag required.
{{% /notice %}}