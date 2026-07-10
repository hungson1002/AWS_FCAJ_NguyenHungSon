---
title : "Thiết lập liên kết IAM OIDC"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.6.1 </b> "
---

### Mục tiêu

Hiểu cách `frontend-hosting.yaml` khai báo IAM OIDC Provider và GitHub Deploy Role cho phép GitHub Actions assume credentials ngắn hạn — không cần lưu AWS Access Key cố định trong repository.

---

#### Cách hoạt động của OIDC

Khi GitHub Actions chạy workflow:
1. GitHub cấp token OIDC tạm thời cho runner.
2. Runner gọi `sts:AssumeRoleWithWebIdentity` để đổi lấy AWS credentials ngắn hạn.
3. Credentials chỉ có hiệu lực trong thời gian workflow chạy.

Điều này loại bỏ rủi ro rò rỉ thông tin xác thực dài hạn (Access Key/Secret Key).

---

#### Cấu hình trong template

Stack `cloud-battleship-frontend-hosting-dev` đã tạo sẵn 2 tài nguyên:
- **GitHubOidcProvider**: Identity Provider kết nối AWS với `token.actions.githubusercontent.com`.
- **GitHubDeployRole**: IAM Role chỉ cho phép `repo:HaoAboutMe/AWS_Cloud_Battleship_Arena` trên nhánh `main` hoặc `develop` assume role. Quyền hạn giới hạn ở S3 sync và CloudFront invalidation.

---

#### Kiểm tra IAM Role đã tạo

```bash
aws iam get-role \
  --role-name cloud-battleship-arena-dev-frontend-github-deploy \
  --region ap-southeast-1
```

Xác nhận `AssumeRolePolicyDocument` chứa điều kiện `StringLike` trỏ đúng đến repo của bạn.

![IAM Role GitHub Deploy](/images/5-Workshop/5.6-CICD-Pipeline/5.6.1-GitHub-OIDC/github-oidc-role.png)
