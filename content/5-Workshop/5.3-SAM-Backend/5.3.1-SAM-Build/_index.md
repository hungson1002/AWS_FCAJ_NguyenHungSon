---
title : "Configure and Build SAM Stack"
date : 2024-01-01 
weight : 1
chapter : false
pre : " <b> 5.3.1 </b> "
---

#### Understanding the template.yaml Structure

The `template.yaml` file is the heart of the Backend. Open it and review the main sections:

```yaml
AWSTemplateFormatVersion: "2010-09-09"
Transform: AWS::Serverless-2016-10-31
Description: Cloud Battleship Arena serverless backend.

Globals:
  Function:
    Runtime: nodejs24.x
    Timeout: 10
    MemorySize: 256
```

**Globals** defines the default configuration for all Lambda functions — avoiding repeated configuration for each function.

#### Step 1: Install Dependencies

Navigate to the `BackEnd/` directory:

```bash
cd AWS_Cloud_Battleship_Arena/BackEnd
npm install
```

#### Step 2: Build SAM Artifact

```bash
sam build
```

The build process will:
1. Read `template.yaml` and identify all Lambda functions.
2. Run `npm install` for each function.
3. Package artifacts into the `.aws-sam/build/` directory.

After a successful build, you'll see:

```
Build Succeeded

Built Artifacts  : .aws-sam/build
Built Template   : .aws-sam/build/template.yaml

Commands you can use next
=========================
[*] Validate SAM template: sam validate
[*] Invoke Function: sam local invoke
[*] Test Function in the Cloud: sam sync --stack-name {{stack-name}} --watch
[*] Deploy: sam deploy --guided
```

#### Step 3: Validate Template (Optional)

```bash
sam validate --lint
```

This command checks the syntax and logic of `template.yaml` to catch errors before deployment.

#### Step 4: Preview Changeset Before Deploying

```bash
sam deploy --guided --no-execute-changeset
```

This shows what changes would be created without actually deploying. Useful for reviewing before applying.

{{% notice tip %}}
After the first `sam deploy --guided` run, SAM automatically saves your parameters to `samconfig.toml`. Subsequent deployments only need `sam deploy` — no `--guided` flag required.
{{% /notice %}}