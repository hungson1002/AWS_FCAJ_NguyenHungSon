---
title : "CloudFront CDN Monitoring"
date : 2024-01-01
weight : 5
chapter : false
pre : " <b> 5.5.5 </b> "
---

### Goals

Deploy the CloudFront monitoring stack to configure error alerting via Email (SNS Alarm) and create a CloudWatch Dashboard for visual traffic monitoring.

---

{{% notice warning %}}
All Amazon CloudFront metrics are published exclusively to the **us-east-1 (N. Virginia)** region. This monitoring stack must be deployed in `us-east-1`.
{{% /notice %}}

---

#### Deploy the Monitoring Stack (AWS CloudFormation Console)

1. **Switch the active region** in the AWS Console to **us-east-1 (N. Virginia)**.
2. Open **CloudFormation → Stacks → Create stack (with new resources)**.
3. Select **Upload a template file** → choose `/Infra/cloudfront-monitoring.yaml`. Click **Next**.
4. **Specify stack details**:
   - **Stack name**: `cloud-battleship-arena-monitoring`.
   - **CloudFrontDistributionId**: `<YOUR_CLOUDFRONT_DISTRIBUTION_ID>` (Enter the CloudFront Distribution ID you copied in step 5.5.2).
   - **MonitoringAlarmEmail**: Enter your personal email address.
   - Click **Next**.
5. Click **Next** and then **Submit** to begin provisioning.

---

#### Confirm SNS Email Subscription

After the stack is created, AWS SNS sends a confirmation email:

1. Open your email inbox.
2. Find the email from **AWS Notifications** with subject `AWS Notification - Subscription Confirmation`.
3. Click **Confirm subscription**.

![SNS Email Subscription Confirmed](/images/5-Workshop/5.5-Frontend-Hosting/5.5.5-CloudFront-Monitoring/sns-confirm.png)

---

#### Verify Monitoring Resources

**CloudWatch Alarms** (ensure region is set to `us-east-1`):

![CloudWatch Alarms active list](/images/5-Workshop/5.5-Frontend-Hosting/5.5.5-CloudFront-Monitoring/cloudfront-alarms.png)

Open **AWS Console → CloudWatch → Alarms**. Confirm 2 CloudFront error rate alarms are displayed.

**CloudWatch Dashboard:**

![CloudWatch Dashboard charts](/images/5-Workshop/5.5-Frontend-Hosting/5.5.5-CloudFront-Monitoring/cloudfront-dashboard.png)

Open **CloudWatch → Dashboards** and select the newly created dashboard to view requests and traffic graphs.
