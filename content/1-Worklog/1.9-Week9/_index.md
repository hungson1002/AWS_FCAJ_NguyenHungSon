---
title: "Week 9 Worklog"
date: 2026-07-05
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### Week 9 Objectives:

* Deploy the web interface to the internet using Amazon S3 and Amazon CloudFront services.
* Compress rank badge and warship images to make the website load faster and run smoother.
* Create a CI/CD automation file with GitHub Actions to redeploy the website whenever code is pushed to the `develop` branch.
* Run tests on all existing game features and fix remaining bugs in the project.
* Design and build a brand new user interface (UI) for all game screens, making them look cleaner, modern, and more attractive.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| 2 | - Deploy the static website on Amazon S3 and configure CloudFront CDN for speed and security <br> - Create an S3 Bucket to hold the Frontend interface code; configure CloudFront to distribute the website and set up security configurations | 06/29/2026 | 06/29/2026 | https://cloudjourney.awsstudygroup.com/ |
| 3 | - Optimize and compress images into WebP format to reduce file size <br> - Compress all rank badge and warship images; replace the old assets with the newly optimized images | 06/30/2026 | 06/30/2026 | https://cloudjourney.awsstudygroup.com/ |
| 4 | - Build automated CI/CD deployment pipelines using GitHub Actions <br> - Write the CI/CD configuration file (`deploy-frontend.yml`) to automatically build and update the web files on S3 and clear the CloudFront cache when new code is pushed to the `develop` branch | 07/01/2026 | 07/01/2026 | https://cloudjourney.awsstudygroup.com/ |
| 5 | - Establish test cases and perform verification across all existing game features <br> - Test the entire system (Cognito login, PvP matchmaking, rank points calculation, sound systems) and fix bugs that occur | 07/02/2026 | 07/02/2026 | https://cloudjourney.awsstudygroup.com/ |
| 6 | - Apply modern design aesthetics and color systems to update game interfaces <br> - Design and implement a completely new interface for the Homepage (Home), Profile page, and ship setup screen | 07/03/2026 | 07/03/2026 | https://cloudjourney.awsstudygroup.com/ |
| 7 | - Sync and configure responsive layouts for mobile and tablet views <br> - Finish the new layout for the PvP battle screen; optimize responsive displays for mobile screens and perform final tests | 07/04/2026 | 07/05/2026 | https://cloudjourney.awsstudygroup.com/ |

### Week 9 Achievements:

* **Static Web Hosting on AWS:**
  * Successfully hosted the Frontend files on S3 and configured CloudFront so users can access the website globally via the internet with fast response times.

* **Optimized Image Assets:**
  * Compressed all warship and rank badge images to WebP format, reducing load times and speed up image rendering.

* **Automated CI/CD Pipeline (GitHub Actions):**
  * Successfully built the workflow file (`deploy-frontend.yml`) to build, deploy static files to S3, and invalidate the CloudFront cache whenever updates are pushed to `develop` or `main`.

* **Testing and Bug Fixing:**
  * Tested all major features and resolved bugs related to connection flows and match synchronization.

* **Complete UI Overhaul:**
  * Replaced the old interface with a modern, eye-catching design featuring polished colors and smooth transitions on the lobby, profile, and PvP game screens.
