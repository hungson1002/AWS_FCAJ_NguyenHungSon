---
title : "Secure Avatar Storage Setup (S3 & CloudFront OAC)"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.5.4 </b> "
---

### Goals

To allow users to upload and view avatar images securely, the system uses **Amazon S3** combined with **Amazon CloudFront** and the **Origin Access Control (OAC)** mechanism.

The S3 Avatar Bucket (`cloud-battleship-backend-dev-avatarbucket-vp9oxrrlmrsu`) is automatically created via the Backend's SAM template. In this step, we will configure this S3 Avatar Bucket as a new **Origin** within the existing **CloudFront Distribution** (`BattleshipArena`) and manually set up Cache Invalidation.

---

### Setup Steps

#### 1. Create Origin Access Control (OAC) for S3 Avatar
1. Go to the **CloudFront Console** > From the left menu, select **Origin access** > Click **Create control setting**.
2. Configure settings:
   - **Name**: `battleship-arena-AvatarOAC`.
   - **Signing behavior**: Select **Sign requests (recommended)**.
   - **Origin type**: Select **S3**.
3. Click **Create**.

---

#### 2. Add S3 Avatar Origin to the Existing CloudFront Distribution
1. Go to the **CloudFront Console** > Click on your existing CloudFront Distribution (e.g., `BattleshipArena` or distribution ID `E1QBTJG476LN8`).
2. Select the **Origins** tab > Click **Create origin**.
3. Configure the origin details as follows:
   - **Origin domain**: Select your S3 Avatar Bucket created from the template (e.g., `cloud-battleship-backend-dev-avatarbucket-vp9oxrrlmrsu.s3.ap-southeast-1.amazonaws.com`).
   - **Origin access**: Select **Origin access control settings (recommended)**.
   - **Origin access control**: Select the `battleship-arena-AvatarOAC` OAC created in the previous step (or click **Create new OAC** if you didn't create it beforehand).

![Create Origin for Avatar Bucket](/images/5-Workshop/5.5-Frontend-Hosting/5.5.4-S3-Avatar-Bucket/create_origin.jpg)

4. Click **Create origin** at the bottom of the page to save the origin.

---

#### 3. Update S3 Bucket Policy
To grant CloudFront OAC permission to read files from the S3 Bucket, update the S3 Bucket Policy:
1. Go to the **Amazon S3 Console** > Select the `cloud-battleship-backend-dev-avatarbucket-vp9oxrrlmrsu` bucket.
2. Select the **Permissions** tab > Scroll to **Bucket policy** and click **Edit**.
3. Paste the following JSON policy (replace with your actual AWS Account ID and CloudFront Distribution ID):

```json
{
    "Version": "2008-10-17",
    "Statement": [
        {
            "Sid": "AllowCloudFrontServicePrincipalReadOnly",
            "Effect": "Allow",
            "Principal": {
                "Service": "cloudfront.amazonaws.com"
            },
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::cloud-battleship-backend-dev-avatarbucket-vp9oxrrlmrsu/*",
            "Condition": {
                "StringEquals": {
                    "AWS:SourceArn": "arn:aws:cloudfront::YOUR_ACCOUNT_ID:distribution/YOUR_CLOUDFRONT_DIST_ID"
                }
            }
        }
    ]
}
```
4. Click **Save changes**.

---

#### 4. Manually Configure CloudFront Cache Invalidation
When players upload a new avatar overwriting their old one (with the same filename e.g. `avatars/userId.jpg`), CloudFront might still serve the cached old image. We must manually create an Invalidation path:
1. Go to the **CloudFront Console** > Select your S3 Avatar Distribution (`BattleshipArena`).
2. Go to the **Invalidations** tab > Click **Create invalidation**.
3. Under **Object paths**, type the paths of the assets to invalidate:
   - Enter `/avatars/*` (to invalidate all avatars) or specifically `/avatars/YOUR_USER_ID.jpg`.
4. Click **Create invalidation** and wait for it to complete.
