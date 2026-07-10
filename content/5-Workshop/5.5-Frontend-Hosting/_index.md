---
title: "Frontend Hosting (S3 & CloudFront)"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

### Goals

Provision a highly optimized and secure static frontend distribution channel for **Cloud Battleship Arena** using **Amazon S3** for static web assets storage (Hosting), **Amazon CloudFront** for global distribution (CDN), and **Origin Access Control (OAC)** to enforce strict access control.

---

#### Why combine S3 + CloudFront + OAC?

1. **Absolute Security**: The S3 Bucket blocks all public access. Users can only fetch assets by requesting them through the CloudFront CDN endpoint.
2. **SPA Compatibility**: Configures Custom Error Responses to translate 403/404 directory errors back to `index.html` to allow seamless client-side routing (React Router).
3. **Low Latency Performance**: HTML, JS, CSS, and media assets are cached at Edge Locations closest to the players.

---

#### Content

- [Create S3 Bucket & Configure OAC](5.5.1-S3-Bucket/)
- [Setup CloudFront Distribution](5.5.2-CloudFront-Distribution/)
- [Secure Avatar Storage Setup (S3 & CloudFront OAC)](5.5.3-S3-Avatar-Bucket/)
- [AWS WAF Edge Security Integration](5.5.4-WAF-Integration/)
- [CloudFront CDN Monitoring](5.5.5-CloudFront-Monitoring/)

