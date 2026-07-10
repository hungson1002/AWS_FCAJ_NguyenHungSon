---
title : "Setup CloudFront Distribution"
date : 2024-01-01
weight : 2
chapter : false
pre : " <b> 5.5.2 </b> "
---

### Goals

Deploy the complete Frontend infrastructure (S3 Bucket, OAC, CloudFront Distribution) via the CloudFormation template `frontend-hosting.yaml` and confirm the game interface loads successfully over HTTPS.

---

#### Custom Error Responses for SPA Routing

The React 19 project uses client-side routing. When a player directly navigates to `/room/ABC` or refreshes the page, S3 returns a `403/404` error because that directory does not physically exist. CloudFront is configured to redirect all such errors to `index.html` with HTTP `200 OK`, allowing React Router to handle the routing correctly.

---

#### Deploy the Stack (AWS CloudFormation Console)

1. Open **AWS Console → CloudFormation → Stacks → Create stack (with new resources)**.
2. **Prepare template**: Select **Template is ready**.
3. **Specify template**: Select **Upload a template file** → choose `/Infra/frontend-hosting.yaml`. Click **Next**.
4. **Specify stack details**:
   - **Stack name**: `cloud-battleship-frontend-hosting-dev`.
   - **GitHubOwner**: `HaoAboutMe`.
   - **GitHubRepo**: `AWS_Cloud_Battleship_Arena`.
   - Click **Next**.
5. **Configure stack options**: Keep defaults. Click **Next**.
6. **Review**: Check **I acknowledge that AWS CloudFormation might create IAM resources** → Click **Submit**.

Provisioning completes in approximately 3–5 minutes.

---

#### Verify the Deployment

Once stack status shows `CREATE_COMPLETE`:

1. Select stack **cloud-battleship-frontend-hosting-dev** → Open the **Outputs** tab.
2. Copy the **CloudFrontDomainName** value (e.g., `d12bu86qwnw1gk.cloudfront.net`).
3. Navigate to your CloudFront URL in a browser and confirm the game interface loads over HTTPS.

![CloudFront Outputs](/images/5-Workshop/5.5-Frontend-Hosting/5.5.2-CloudFront-Distribution/cloudfront-domain-output.png)

