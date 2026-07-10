---
title : "Setup IAM OIDC Integration"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.6.1 </b> "
---

### Goals

Understand how `frontend-hosting.yaml` declares the IAM OIDC Provider and GitHub Deploy Role, enabling GitHub Actions to assume short-lived AWS credentials — eliminating the need to store long-term Access Keys in the repository.

---

#### How OIDC Works

When GitHub Actions runs a workflow:
1. GitHub issues a temporary OIDC token to the runner.
2. The runner calls `sts:AssumeRoleWithWebIdentity` to exchange it for short-lived AWS credentials.
3. Credentials expire automatically when the workflow finishes.

This eliminates the risk of long-term credential (Access Key/Secret Key) leaks.

---

#### Configuration in the Template

The `cloud-battleship-frontend-hosting-dev` stack has already created 2 resources:
- **GitHubOidcProvider**: Identity Provider connecting AWS to `token.actions.githubusercontent.com`.
- **GitHubDeployRole**: IAM Role allowing only `repo:HaoAboutMe/AWS_Cloud_Battleship_Arena` on the `main` or `develop` branch to assume the role. Permissions are limited to S3 sync and CloudFront cache invalidation.

---

#### Verify the Deployed IAM Role

```bash
aws iam get-role \
  --role-name cloud-battleship-arena-dev-frontend-github-deploy \
  --region ap-southeast-1
```

Confirm that the `AssumeRolePolicyDocument` contains a `StringLike` condition pointing to your repository.

![IAM Role GitHub Deploy](/images/5-Workshop/5.6-CICD-Pipeline/5.6.1-GitHub-OIDC/github-oidc-role.png)
