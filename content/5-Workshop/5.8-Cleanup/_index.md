---
title: "Resource Cleanup"
date: 2024-01-01
weight: 8
chapter: false
pre: " <b> 5.8. </b> "
---

### Goals

Delete all Backend and Frontend resources deployed on AWS to prevent unexpected billing charges.

{{% notice warning %}}
**Cost reminder**: AWS WAF incurs a fixed charge of **~$6.00/month** ($5/Web ACL + $1/Rule) even with zero traffic. Delete the WAF Web ACL **first** before removing other resources.
{{% /notice %}}

---

#### Step 1: Delete Backend Stack (SAM CLI)

```bash
cd BackEnd
sam delete --stack-name cloud-battleship-backend-dev
```

Enter `y` when prompted to confirm deletion.

---

#### Step 2: Delete Monitoring Stack (CloudFormation Console — us-east-1)

1. **Switch the region** to **us-east-1**.
2. Open **CloudFormation → Stacks**.
3. Select stack **cloud-battleship-arena-monitoring** → Click **Delete** → Confirm.

---

#### Step 3: Delete Frontend Stack (CloudFormation Console)

1. Switch the region back to **ap-southeast-1**.
2. Open **CloudFormation → Stacks**.
3. Select stack **cloud-battleship-frontend-hosting-dev** → Click **Delete** → Confirm.
4. Wait for the status to transition to `DELETE_COMPLETE`.

---

#### Step 4: Delete AWS WAF Web ACL (WAF Console — Global)

{{% notice warning %}}
The WAF Web ACL is scoped to **Global (CloudFront)** — you must switch to the **us-east-1** region to see it.
{{% /notice %}}

1. **Switch the region** to **us-east-1 (N. Virginia)**.
2. Open **AWS WAF & Shield Console → Web ACLs**.
3. Select the **CloudBattleship-WAF** Web ACL.
4. Go to the **Associated AWS resources** tab → Confirm no CloudFront Distributions are still attached (if any remain, click **Remove** to detach them first).
5. Return to the **Web ACLs** list → Select **CloudBattleship-WAF** → Click **Delete** → Confirm.

---

#### Step 5: Delete Cognito User Pool (Cognito Console)

1. Open **AWS Console → Cognito → User pools**.
2. Select the **Battleship-Arena** User Pool.
3. Click **Delete user pool** → Type the pool name to confirm → Click **Delete**.

---

#### Cleanup Checklist

- [ ] Stack `cloud-battleship-backend-dev` deleted successfully.
- [ ] Stack `cloud-battleship-frontend-hosting-dev` deleted successfully.
- [ ] Stack `cloud-battleship-arena-monitoring` deleted successfully.
- [ ] S3 Bucket emptied and deleted.
- [ ] CloudFront Distribution disabled and removed.
- [ ] Web ACL `CloudBattleship-WAF` fully deleted (verify in us-east-1).
- [ ] Cognito User Pool `Battleship-Arena` fully deleted.

---

#### Summary

Congratulations on completing the full hands-on workshop! You have successfully implemented real-world Cloud technologies:

| Topic | Technology Applied |
|---|---|
| **Infrastructure as Code** | AWS SAM, AWS CloudFormation |
| **Serverless Compute** | AWS Lambda (Node.js 24.x) |
| **REST & Real-time API** | Amazon API Gateway (HTTP & WebSocket) |
| **NoSQL Database** | Amazon DynamoDB (TTL, GSI, On-demand) |
| **Static Hosting & CDN** | Amazon S3 + Amazon CloudFront (OAC) |
| **Automated CI/CD** | GitHub Actions + AWS OpenID Connect |
| **Cloud Security** | IAM Least Privilege, Cognito JWT Auth, AWS WAF |