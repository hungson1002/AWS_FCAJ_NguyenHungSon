---
title : "Cấu hình Workflow deploy-frontend.yml"
date : 2024-01-01
weight : 2
chapter : false
pre : " <b> 5.6.2 </b> "
---

### Mục tiêu

Cấu hình tệp workflow GitHub Actions, thiết lập Variables/Secrets trên repository và chạy thử pipeline CI/CD tự động hóa.

---

#### Bước 1: Khởi tạo tệp Workflow

Tạo một tệp tin tại đường dẫn `.github/workflows/deploy-frontend.yml` trong mã nguồn dự án của bạn:

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
Tệp JS/CSS trong `/assets/` có hash ngẫu nhiên trong tên → cache vĩnh viễn (`immutable`, 1 năm). Tệp `index.html` không cache (`no-store`) để trình duyệt luôn lấy phiên bản mới nhất.
{{% /notice %}}

---

#### Bước 2: Cấu hình biến môi trường trên GitHub

1. Mở repo GitHub của bạn, chọn **Settings → Secrets and variables → Actions**.
2. Thêm các **Repository variables**:
   - `AWS_REGION`: `ap-southeast-1`
   - `FRONTEND_BUCKET`: `cloud-battleship-frontend-hosting-d-frontendbucket-41a7wzdtapv0`
   - `CLOUDFRONT_DISTRIBUTION_ID`: `E2NO3LKHKC2CLD`
   - Các biến cấu hình Frontend (ví dụ: `VITE_AWS_REGION` là `ap-southeast-1`, `VITE_AWS_USER_POOL_ID` là `ap-southeast-1_VV7CeCaWL`, v.v.).
3. Thêm một **Repository secret**:
   - `AWS_DEPLOY_ROLE_ARN`: `arn:aws:iam::670516425271:role/cloud-battleship-arena-dev-frontend-github-deploy`

**Repository Secrets:**

![Cấu hình Secrets trên GitHub](/images/5-Workshop/5.6-CICD-Pipeline/5.6.2-Deploy-Workflow/github-repository-secrets.png)

**Repository Variables:**

![Cấu hình Variables trên GitHub](/images/5-Workshop/5.6-CICD-Pipeline/5.6.2-Deploy-Workflow/github-repository-variables.png)

---

#### Bước 3: Chạy Workflow

1. Đẩy mã nguồn của bạn lên nhánh `develop` hoặc `main`:
   ```bash
   git add .
   git commit -m "Configure CI/CD deploy pipeline"
   git push origin develop
   ```
2. Mở tab **Actions** trên GitHub và xác nhận workflow tự động chạy.

![GitHub Actions workflow chạy thành công](/images/5-Workshop/5.6-CICD-Pipeline/5.6.2-Deploy-Workflow/github-actions-run.png)

---

#### Kiểm tra kết quả

**CloudFront Invalidation (Xóa cache):**

![CloudFront Invalidation hoàn tất](/images/5-Workshop/5.6-CICD-Pipeline/5.6.2-Deploy-Workflow/cloudfront-invalidation.png)

Kiểm tra tab **Invalidations** trong CloudFront Distribution. Xác nhận có bản ghi invalidation cho đường dẫn `/*` đã hoàn thành (`Completed`).
