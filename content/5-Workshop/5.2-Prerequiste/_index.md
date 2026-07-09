---
title : "Environment Setup"
date : 2024-01-01 
weight : 2 
chapter : false
pre : " <b> 5.2. </b> "
---

#### System Requirements

Before starting the workshop, make sure you have installed and configured the following tools:

| Tool | Version | Purpose |
|---|---|---|
| **Node.js** | v20+ | Lambda and Frontend runtime |
| **AWS CLI** | v2 | Interact with AWS from terminal |
| **AWS SAM CLI** | latest | Build and deploy serverless stack |
| **Git** | any | Source code management |

#### Step 1: Install AWS CLI

Download and install AWS CLI v2 from the official AWS website. After installation, configure your credentials:

```bash
aws configure
```

Enter the following when prompted:
```
AWS Access Key ID [None]: <your-access-key>
AWS Secret Access Key [None]: <your-secret-key>
Default region name [None]: ap-southeast-1
Default output format [None]: json
```

#### Step 2: Install AWS SAM CLI

```bash
# Windows (use MSI installer from AWS SAM page)
# Or use pip:
pip install aws-sam-cli
```

Verify installation:
```bash
sam --version
```

#### Step 3: Set Up IAM Permissions

Attach the following IAM permission policy to your IAM user/role to have sufficient permissions to deploy and clean up resources in this workshop:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "BattleshipWorkshopPermissions",
            "Effect": "Allow",
            "Action": [
                "cloudformation:*",
                "cloudwatch:*",
                "cognito-idp:*",
                "dynamodb:*",
                "apigateway:*",
                "lambda:*",
                "s3:*",
                "iam:CreateRole",
                "iam:DeleteRole",
                "iam:AttachRolePolicy",
                "iam:DetachRolePolicy",
                "iam:PutRolePolicy",
                "iam:DeleteRolePolicy",
                "iam:GetRole",
                "iam:GetRolePolicy",
                "iam:PassRole",
                "iam:ListRoles",
                "iam:CreateInstanceProfile",
                "iam:DeleteInstanceProfile",
                "iam:AddRoleToInstanceProfile",
                "iam:RemoveRoleFromInstanceProfile",
                "logs:*",
                "sns:*",
                "ssm:GetParameters"
            ],
            "Resource": "*"
        }
    ]
}
```

#### Step 4: Clone the Project Source Code

```bash
git clone https://github.com/hungson1002/AWS_Cloud_Battleship_Arena.git
cd AWS_Cloud_Battleship_Arena
```

#### Step 5: Create Cognito User Pool (Prerequisite)

The backend requires an existing Cognito User Pool. Create one via the AWS Console or CLI:

```bash
# Create User Pool
aws cognito-idp create-user-pool \
  --pool-name "CloudBattleshipArena" \
  --auto-verified-attributes email \
  --region ap-southeast-1

# Create App Client (note down UserPoolId and ClientId)
aws cognito-idp create-user-pool-client \
  --user-pool-id <USER_POOL_ID> \
  --client-name "battleship-client" \
  --no-generate-secret \
  --region ap-southeast-1
```

Note down these two important values:
- `UserPoolId` — Example: `ap-southeast-1_XXXXXXXXX`
- `ClientId` — Example: `1abc2defgh3ijklmnop4qrstu`

You will need them in the next step when deploying the Backend with SAM.