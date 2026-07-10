---
title : "Configure deploy-frontend.yml Workflow"
date : 2024-01-01
weight : 2
chapter : false
pre : " <b> 5.6.2 </b> "
---

### Goals

Configure the GitHub Actions workflow file, set up Secrets and Variables on the repository, and run the automated CI/CD pipeline.

---

#### Step 1: Create the Workflow File

Create a YAML configuration file under `.github/workflows/deploy-frontend.yml` in your project root:

```yaml
name: Deploy Frontend

on:
  push:
    branches:
      - main
      - develop
    paths:
      - "FrontEnd/**"
      - ".github/workflows/deploy-frontend.yml"
  workflow_dispatch:

permissions:
  contents: read
  id-token: write

concurrency:
  group: frontend-production
  cancel-in-progress: true

env:
  NODE_VERSION: "24"
  AWS_REGION: ${{ vars.AWS_REGION }}
  FRONTEND_BUCKET: ${{ vars.FRONTEND_BUCKET }}
  CLOUDFRONT_DISTRIBUTION_ID: ${{ vars.CLOUDFRONT_DISTRIBUTION_ID }}

jobs:
  deploy:
    name: Build and deploy
    runs-on: ubuntu-latest

    defaults:
      run:
        working-directory: FrontEnd

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Validate deployment configuration
        run: |
          test -n "${AWS_REGION}" || (echo "Missing repository variable: AWS_REGION" && exit 1)
          test -n "${FRONTEND_BUCKET}" || (echo "Missing repository variable: FRONTEND_BUCKET" && exit 1)
          test -n "${CLOUDFRONT_DISTRIBUTION_ID}" || (echo "Missing repository variable: CLOUDFRONT_DISTRIBUTION_ID" && exit 1)
          test -n "${AWS_DEPLOY_ROLE_ARN}" || (echo "Missing repository secret: AWS_DEPLOY_ROLE_ARN" && exit 1)
        working-directory: .
        env:
          AWS_DEPLOY_ROLE_ARN: ${{ secrets.AWS_DEPLOY_ROLE_ARN }}

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: npm
          cache-dependency-path: FrontEnd/package-lock.json

      - name: Install dependencies
        run: npm ci

      - name: Build frontend
        run: npm run build
        env:
          VITE_AWS_REGION: ${{ vars.VITE_AWS_REGION }}
          VITE_AWS_USER_POOL_ID: ${{ vars.VITE_AWS_USER_POOL_ID }}
          VITE_AWS_USER_POOL_CLIENT_ID: ${{ vars.VITE_AWS_USER_POOL_CLIENT_ID }}
          VITE_AWS_COGNITO_DOMAIN: ${{ vars.VITE_AWS_COGNITO_DOMAIN }}
          VITE_AWS_OAUTH_REDIRECT_SIGN_IN: ${{ vars.VITE_AWS_OAUTH_REDIRECT_SIGN_IN }}
          VITE_AWS_OAUTH_REDIRECT_SIGN_OUT: ${{ vars.VITE_AWS_OAUTH_REDIRECT_SIGN_OUT }}
          VITE_API_BASE_URL: ${{ vars.VITE_API_BASE_URL }}
          VITE_WS_BASE_URL: ${{ vars.VITE_WS_BASE_URL }}
          VITE_CLOUDFRONT_DOMAIN: ${{ vars.VITE_CLOUDFRONT_DOMAIN }}
          VITE_FRONTEND_APP_URL: ${{ vars.VITE_FRONTEND_APP_URL }}

      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          role-to-assume: ${{ secrets.AWS_DEPLOY_ROLE_ARN }}
          aws-region: ${{ env.AWS_REGION }}

      - name: Upload public static files
        run: >
          aws s3 sync dist/ "s3://${FRONTEND_BUCKET}/"
          --delete
          --exclude "index.html"
          --exclude "assets/*"
          --cache-control "public,max-age=3600"

      - name: Upload immutable build assets
        run: >
          aws s3 sync dist/assets/ "s3://${FRONTEND_BUCKET}/assets/"
          --delete
          --cache-control "public,max-age=31536000,immutable"

      - name: Upload SPA entrypoint
        run: >
          aws s3 cp dist/index.html "s3://${FRONTEND_BUCKET}/index.html"
          --cache-control "no-cache,no-store,must-revalidate"
          --content-type "text/html"

      - name: Invalidate CloudFront cache
        run: >
          aws cloudfront create-invalidation
          --distribution-id "${CLOUDFRONT_DISTRIBUTION_ID}"
          --paths "/*"
```

{{% notice note %}}
JS/CSS files in `/assets/` contain content hashes in their filenames → cache permanently (`immutable`, 1 year). The `index.html` file must never be cached (`no-store`) so browsers always fetch the latest version.
{{% /notice %}}

---

#### Step 2: Configure Environment Parameters on GitHub

1. Navigate to your GitHub repository, then open **Settings → Secrets and variables → Actions**.
2. Create the following **Repository variables**:
   - `AWS_REGION`: `ap-southeast-1`
   - `FRONTEND_BUCKET`: `cloud-battleship-frontend-hosting-d-frontendbucket-41a7wzdtapv0`
   - `CLOUDFRONT_DISTRIBUTION_ID`: `E2NO3LKHKC2CLD`
   - Additional environment configurations (like `VITE_AWS_REGION` as `ap-southeast-1`, `VITE_AWS_USER_POOL_ID` as `ap-southeast-1_VV7CeCaWL`, etc.).
3. Create the following **Repository secret**:
   - `AWS_DEPLOY_ROLE_ARN`: `arn:aws:iam::670516425271:role/cloud-battleship-arena-dev-frontend-github-deploy`

**Repository Secrets:**

![Configure Secrets in GitHub Repo Settings](/images/5-Workshop/5.6-CICD-Pipeline/5.6.2-Deploy-Workflow/github-repository-secrets.png)

**Repository Variables:**

![Configure Variables in GitHub Repo Settings](/images/5-Workshop/5.6-CICD-Pipeline/5.6.2-Deploy-Workflow/github-repository-variables.png)

---

#### Step 3: Trigger the Workflow

1. Push your configurations to the `develop` or `main` branch:
   ```bash
   git add .
   git commit -m "Configure CI/CD deploy pipeline"
   git push origin develop
   ```
2. Navigate to the **Actions** tab in your repository and verify the deployment status.

![GitHub Actions workflow run completed successfully](/images/5-Workshop/5.6-CICD-Pipeline/5.6.2-Deploy-Workflow/github-actions-run.png)

---

#### Verify the Deployment

**CloudFront Invalidation (Cache Purge):**

![CloudFront Invalidation completed](/images/5-Workshop/5.6-CICD-Pipeline/5.6.2-Deploy-Workflow/cloudfront-invalidation.png)

Open the **Invalidations** tab in your CloudFront Distribution. Confirm an invalidation record for path `/*` has a status of `Completed`.
