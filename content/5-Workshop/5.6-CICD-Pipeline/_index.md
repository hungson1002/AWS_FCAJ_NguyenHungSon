---
title: "CI/CD Automation with GitHub Actions"
date: 2026-07-10
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

### Goals

Set up a fully automated CI/CD pipeline for the Frontend application. Whenever you push new commits to the `main` or `develop` branch, GitHub Actions will trigger, install dependencies, build the React bundle, deploy assets to Amazon S3, and invalidate the CloudFront CDN cache.

---

#### Why Use IAM OpenID Connect (OIDC) for GitHub Actions?

Using **OpenID Connect (OIDC)** provides a modern, secure alternative:
1. **No Long-lived Credentials**: GitHub Actions authenticates with AWS dynamically using short-lived Web Identity Tokens.
2. **Ephemeral Access**: Security credentials automatically expire once the deployment job finishes.
3. **Scoping Restrictions**: Trust relationships restrict deployment privileges only to specific branches (`main` or `develop`) in your authorized repository.

---

#### Content

- [Setup IAM OIDC Integration](5.6.1-GitHub-OIDC/)
- [Configure deploy-frontend.yml Workflow](5.6.2-Deploy-Workflow/)
