---
title : "Amazon Cognito Setup"
date : 2024-01-01 
weight : 2
chapter : false
pre : " <b> 5.2.2. </b> "
---

### Goals

Provision an Amazon Cognito User Pool directly from the AWS Console to handle user authentication for the Cloud Battleship Arena application.

---

#### Step 1: Create the Cognito User Pool

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

---

#### Step 2: Save User Pool Credentials

Navigate inside your newly created pool and save these two values:
- **User Pool ID** (formatted as `<YOUR_COGNITO_USER_POOL_ID>`)
- **Client ID** (formatted as `<YOUR_COGNITO_USER_POOL_CLIENT_ID>`)

<img src="/images/5-Workshop/5.2-Prerequisite/cognito-app-client.png" width="100%">

> 📌 These values will be used to configure the Frontend environment variables in step 5.5.
