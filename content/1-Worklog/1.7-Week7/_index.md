---
title: "Week 7 Worklog"
date: 2026-06-20
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Week 7 Objectives:

* Finalize real-time communication for PvP gameplay by integrating API Gateway WebSockets.
* Complete the user profile picture (avatar) upload feature integrated with AWS S3 storage.
* Complete the user Profile page, displaying comprehensive player statistics and detailed match history.
* Add immersive sound effects for game interactions and optimize the audio asset loading process.
* Achieve comprehensive Responsive design compatibility for all screens in the game.
* Fix Lambda and API Gateway errors, and refactor the match history recording process to be handled on the Server-side for data integrity.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date |
| --- | ---- | ---------- | --------------- |
| 2 | - Study and configure API Gateway WebSocket APIs with default routes (`$connect`, `$disconnect`) and custom routes (`joinRoom`, `makeMove`) <br> - Develop Lambda functions to manage session and connection ID mapping for tracking online players | 06/15/2026 | 06/15/2026 |
| 3 | - Connect WebSocket on the Frontend and implement real-time game state synchronization <br> - Integrate sound effects (explosion hits, water splashes, background music) <br> - Resolve browser autoplay policy issues by initializing audio securely via user interaction | 06/16/2026 | 06/16/2026 |
| 4 | - Implement user avatar upload feature using AWS S3 <br> - Write a Lambda Function to generate S3 Presigned URLs so the Frontend can securely upload images directly to the S3 bucket <br> - Configure S3 Bucket IAM Policies and CORS to enable smooth asset uploading and rendering | 06/17/2026 | 06/17/2026 |
| 5 | - Enhance the user Profile page by rendering dynamic avatars from S3 and updating profile metadata in DynamoDB <br> - Complete the visualization of player metrics (Service Record) and match logs in an intuitive format | 06/18/2026 | 06/18/2026 |
| 6 | - Test, debug, and fix API Gateway CORS issues and Lambda function timeouts <br> - Design and implement a secure, optimized solution: transition the match history persistence (`saveMatchHistory`) from a client-triggered API call to an automated server-side write upon WebSocket disconnect/match completion | 06/19/2026 | 06/19/2026 |
| 7 | - Optimize and complete Responsive design for all game screens (Lobby, ship setup, battle board, profile) <br> - Test WebSocket connection resilience during network disruptions and finalize the Week 7 report | 06/20/2026 | 06/21/2026 |

### Week 7 Achievements:

* **Real-time Online PvP Implementation (WebSocket):**
  * Successfully deployed API Gateway WebSocket API integrated with AWS Lambda to manage active connections and online matchmaking.
  * Synchronized game events in real-time (turns, coordinates fired, ship sinking), delivering a lag-free PvP gaming experience.

* **Completed Avatar Uploading via AWS S3:**
  * Implemented a secure upload mechanism utilizing S3 Presigned URLs, minimizing overhead on backend servers.
  * Programmed automatic profile updates in DynamoDB to reference the newly uploaded avatar S3 URLs across the application.

* **Polished User Profile Page:**
  * Connected frontend profile widgets to live DynamoDB statistics, rendering total games, wins, losses, and shooting accuracy metrics (shots, hits, misses).
  * Styled a clean match history table featuring opponent avatars, game mode indicator (rank/casual), win/loss status, and match timestamps.

* **Enhanced Game Audio & Sound Effects:**
  * Embedded high-quality audio triggers for game events: hits, misses, ship destruction, and game-over screens.
  * Resolved asynchronous audio buffering issues to optimize initial page load speed and eliminate audio stuttering on mobile viewports.

* **Comprehensive Responsive Design:**
  * Ensured the game renders flawlessly on mobile and tablet form factors with adaptive CSS layout rules.
  * Fixed visual bugs, text overlapping, and board grid warping on smaller screen sizes.

* **System Debugging and Security Hardening:**
  * Eliminated API Gateway integration issues (CORS configuration, JSON payload parsing, and Lambda timeout limits).
  * **Secure Data Management Solution:** Closed the security vulnerability of client-controlled match history saves. The server now calculates the match outcome from WebSocket connection states and updates DynamoDB directly, preventing malicious score tampering.
