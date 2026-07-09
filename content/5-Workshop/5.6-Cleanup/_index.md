---
title : "Clean Up Resources"
date : 2024-01-01
weight : 6
chapter : false
pre : " <b> 5.6. </b> "
---

Congratulations on completing the **Cloud Battleship Arena Serverless Backend** workshop!

In this workshop, you learned and practiced:
+ **AWS SAM** — Define and deploy Serverless infrastructure as code (IaC).
+ **AWS Lambda** — Build serverless game logic with Node.js 24.x.
+ **API Gateway HTTP API** — Provide RESTful endpoints for room management and user profiles with Cognito JWT Authorizer.
+ **API Gateway WebSocket API** — Create a real-time bidirectional channel for gameplay.
+ **Amazon DynamoDB** — Store game state with on-demand billing and automatic TTL.
+ **Amazon Cognito** — Protect APIs with JWT tokens and the Least Privilege principle.

#### Clean Up

To avoid unnecessary costs, delete all resources created during this workshop.

**Step 1: Delete the SAM Backend Stack**

```bash
cd BackEnd
sam delete --stack-name cloud-battleship-backend
```

SAM will ask for confirmation before deleting. Type `y` to confirm. The deletion process takes approximately 2–3 minutes.

**Step 2: Delete the S3 Bucket for SAM Artifacts**

SAM creates an S3 bucket to store deployment artifacts. Delete it manually:

```bash
# Find the bucket name (usually prefixed with "aws-sam-cli-managed-default")
aws s3 ls | grep aws-sam-cli

# Remove all objects from the bucket
aws s3 rm s3://<bucket-name> --recursive

# Delete the bucket
aws s3 rb s3://<bucket-name>
```

**Step 3: Delete the Cognito User Pool**

```bash
# Delete the App Client first
aws cognito-idp delete-user-pool-client \
  --user-pool-id <USER_POOL_ID> \
  --client-id <CLIENT_ID>

# Delete the User Pool
aws cognito-idp delete-user-pool \
  --user-pool-id <USER_POOL_ID>
```

**Step 4: Verify in the CloudFormation Console**

Open **AWS Console → CloudFormation → Stacks** and confirm that the `cloud-battleship-backend` stack has been fully deleted (status: `DELETE_COMPLETE`).

**Step 5: Verify in the DynamoDB Console**

Open **AWS Console → DynamoDB → Tables** and confirm that the following tables have been deleted:
+ `Rooms`
+ `Connections`
+ `ChatMessages`

**Step 6: Verify in the Lambda Console**

Open **AWS Console → Lambda → Functions** and confirm there are no remaining functions from this workshop.

#### Note

> ⚠️ If you separately deployed a Frontend stack (`frontend-hosting.yaml`), delete that stack in the CloudFormation Console or run `aws cloudformation delete-stack --stack-name <frontend-stack-name>`.