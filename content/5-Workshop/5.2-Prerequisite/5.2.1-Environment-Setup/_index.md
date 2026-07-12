---
title : "Environment Setup"
date : 2026-07-10 
weight : 1
chapter : false
pre : " <b> 5.2.1. </b> "
---

### Goals

Create an IAM User with appropriate permissions, generate an Access Key for AWS CLI authentication, install the required CLI tools, and clone the project source code.

---

#### Tool Requirements

| Tool | Version | Purpose |
|---|---|---|
| **Node.js** | v20+ | Runtime for Lambda functions and Frontend build |
| **AWS CLI** | v2 | Interact with AWS resources from the terminal |
| **AWS SAM CLI** | latest | Build & deploy the Serverless Backend stack |
| **Git** | any | Source code version control |

---

#### Step 1: Create an IAM User

1. Open the **AWS Console** and navigate to the **IAM** service.

<img src="/images/5-Workshop/5.2-Prerequisite/5.2.1-Environment-Setup/access-iam.jpg" width="100%">

2. In the left menu, select **Users** → click **Create user**.

<img src="/images/5-Workshop/5.2-Prerequisite/5.2.1-Environment-Setup/createUser.jpg" width="100%">

3. Enter a **User name** (e.g. `battleship-deployer`). Check **Provide user access to the AWS Management Console** to grant Console login access and set a password. Click **Next**.

<img src="/images/5-Workshop/5.2-Prerequisite/5.2.1-Environment-Setup/setup-username-password.jpg" width="100%">

4. On the **Set permissions** step, select **Attach policies directly**. Search for and check **AdministratorAccess**. Click **Next**.

<img src="/images/5-Workshop/5.2-Prerequisite/5.2.1-Environment-Setup/setup-policy.jpg" width="100%">

5. Review all details on the **Review and create** page. Click **Create user**.

<img src="/images/5-Workshop/5.2-Prerequisite/5.2.1-Environment-Setup/review.jpg" width="100%">

6. User created successfully. Click **Download .csv file** to save the Console login credentials (username, password, sign-in URL).

> ⚠️ The CSV file can only be downloaded **once** at this point. Store it in a safe location before closing the page.

<img src="/images/5-Workshop/5.2-Prerequisite/5.2.1-Environment-Setup/success.jpg" width="100%">

---

#### Step 2: Create an Access Key

1. Navigate to the newly created user’s detail page → **Security credentials** tab.
2. Scroll down to the **Access keys** section → click **Create access key**.

<img src="/images/5-Workshop/5.2-Prerequisite/5.2.1-Environment-Setup/createAccessKey.jpg" width="100%">

3. Select the **Command Line Interface (CLI)** use case → confirm → click **Next**.
4. Enter a description tag (optional) → click **Create access key**.

<img src="/images/5-Workshop/5.2-Prerequisite/5.2.1-Environment-Setup/SetDescriptionTag.jpg" width="100%">

5. **Save immediately**:
   - **Access Key ID**
   - **Secret Access Key**

> ⚠️ The Secret Access Key is shown **only once**. Store it in a safe place before closing the page.

<img src="/images/5-Workshop/5.2-Prerequisite/5.2.1-Environment-Setup/ReviewAccessKey.jpg" width="100%">

---

#### Step 3: Configure AWS CLI

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

#### Step 4: Clone the Project Repository

```bash
git clone https://github.com/HaoAboutMe/AWS_Cloud_Battleship_Arena.git
cd AWS_Cloud_Battleship_Arena
```
