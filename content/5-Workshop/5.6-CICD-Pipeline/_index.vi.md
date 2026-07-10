---
title: "Tự động hóa CI/CD với GitHub Actions"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

### Mục tiêu

Thiết lập một đường ống CI/CD tự động hóa hoàn chỉnh cho phần Frontend. Mỗi khi bạn đẩy code mới lên nhánh `main` hoặc `develop`, GitHub Actions sẽ tự động kiểm tra, cài đặt dependencies, biên dịch (Build) dự án React và đồng bộ hóa (Deploy) lên AWS S3, đồng thời xóa cache cũ trên CloudFront.

---

#### Tại sao sử dụng IAM OpenID Connect (OIDC) cho GitHub Actions?

Với **OpenID Connect (OIDC)**:
1. **Không dùng khóa dài hạn**: GitHub Actions authenticate với AWS thông qua giao thức token ngắn hạn (Web Identity Token).
2. **Quyền truy cập ngắn hạn**: Token tự động hết hạn sau khi bước deploy hoàn tất.
3. **Phân quyền chặt chẽ**: AWS chỉ tin cậy các token được phát từ repo GitHub của bạn và chạy trên các nhánh cụ thể (`main` hoặc `develop`).

---

#### Nội dung bài học

- [Thiết lập liên kết IAM OIDC (AWS & GitHub)](5.6.1-GitHub-OIDC/)
- [Cấu hình Workflow deploy-frontend.yml](5.6.2-Deploy-Workflow/)
