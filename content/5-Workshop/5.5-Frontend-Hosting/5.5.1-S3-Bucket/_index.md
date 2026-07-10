---
title : "Create S3 Bucket & Configure OAC"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.5.1 </b> "
---

### Goals

Understand how the IaC template `frontend-hosting.yaml` defines a secure S3 Bucket and the OAC mechanism that allows CloudFront to serve content without exposing the bucket publicly.

---

#### How S3 + OAC Works

The project's Frontend infrastructure is managed through `/Infra/frontend-hosting.yaml`:

- **S3 Bucket**: Declared with `PublicAccessBlockConfiguration` fully enabled — blocking all direct internet access.
- **Origin Access Control (OAC)**: Signs SigV4 headers on each CloudFront request to S3 as proof of identity.
- **S3 Bucket Policy**: Grants `s3:GetObject` permission exclusively to your project's CloudFront distribution.

All 3 resources are created automatically when the CloudFormation stack is deployed in step 5.5.2.

---

#### Verify Configuration After Deployment

After the `cloud-battleship-frontend-hosting-dev` stack is successfully created, confirm S3 blocks are active:

```bash
aws s3api get-public-access-block \
  --bucket cloud-battleship-frontend-hosting-d-frontendbucket-41a7wzdtapv0 \
  --region ap-southeast-1
```

All four fields must return `true`:
```json
{
    "PublicAccessBlockConfiguration": {
        "BlockPublicAcls": true,
        "IgnorePublicAcls": true,
        "BlockPublicPolicy": true,
        "RestrictPublicBuckets": true
    }
}
```

---

#### Verify in S3 Console

![S3 Block Public Access](/images/5-Workshop/5.5-Frontend-Hosting/5.5.1-S3-Bucket/public-access-block.png)

