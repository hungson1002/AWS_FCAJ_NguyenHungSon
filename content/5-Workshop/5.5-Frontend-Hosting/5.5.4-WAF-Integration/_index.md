---
title : "AWS WAF Edge Security Integration"
date : 2026-07-10
weight : 4
chapter : false
pre : " <b> 5.5.4 </b> "
---

### Goals

Protect the static game frontend assets and mitigate basic DDoS or brute-force attacks by attaching **AWS WAF (Web Application Firewall)** to the **CloudFront Distribution** hosting the frontend.

---

### Steps to Configure AWS WAF

#### 1. Create a New Web ACL (Access Control List)
1. Go to the **AWS WAF Console** > Select **Web ACLs** from the left menu.
2. Under **Region**, select **Global (CloudFront)** (This is required since CloudFront is a global edge service).
3. Click **Create web ACL**.
4. Configure basic details:
   - **Name**: `CloudBattleship-WAF`.
5. Skip associating resources in the initial creation wizard steps, click **Next** through Rule configuration and Metrics, then click **Create web ACL** to complete the creation.

---

#### 2. Associate the Web ACL with CloudFront Distribution (Post-Creation)
Once the Web ACL `CloudBattleship-WAF` is successfully created, proceed to associate your resources:
1. In the Web ACLs list, click on the name `CloudBattleship-WAF` that you just created.
2. Select the **Associated AWS resources** tab > Click the **Add global resources** button.
3. Select **Add CloudFront or Amplify resources** from the dropdown menu.

![Manage Protected Resources](/images/5-Workshop/5.5-Frontend-Hosting/5.5.4-WAF-Integration/manage_resource.png)

4. In the **Select resources to protect** window, check your frontend's CloudFront Distribution (e.g., `<YOUR_CLOUDFRONT_DISTRIBUTION_ID> - <YOUR_CLOUDFRONT_DOMAIN_NAME> - cloud-battleship-arena dev frontend`).

![Associate CloudFront Distribution](/images/5-Workshop/5.5-Frontend-Hosting/5.5.4-WAF-Integration/select_resource.png)

5. Click **Add** to complete the association.

---

#### 3. Create a Rate-based Rule
To prevent request spamming from a single IP address:
1. Inside the Web ACL details page for `CloudBattleship-WAF`, select the **Rules** tab > click **Add rules** > Select **Add my own rules and rule groups**.
2. In the right pane "Add new rule", select **Custom rule** and click **Next**.

![Select Custom Rule](/images/5-Workshop/5.5-Frontend-Hosting/5.5.4-WAF-Integration/manage_rule.png)

3. In the detail configuration screen, select the rule type and configure the following:
   - **Rule type**: Select **Rate-based rule**.
   - **Rule name**: Set a name (e.g., `SpamLimitRule`).
   - **Rate limit**: Enter `100` (Limits each IP to a maximum of 100 requests per 5 minutes).
   - **IP address to use for rate limiting**: Select **Source IP address**.
   - **Action**: Select **Block** to block requests exceeding the threshold.

![Configure Rate-based Rule](/images/5-Workshop/5.5-Frontend-Hosting/5.5.4-WAF-Integration/set_based_limit.png)

4. Click **Add rule** at the bottom of the screen and save your changes.
