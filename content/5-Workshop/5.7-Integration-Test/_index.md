---
title : "Integration Test"
date : 2024-01-01
weight : 7
chapter : false
pre : " <b> 5.7. </b> "
---

### Goals

Validate the entire application flow by registering user accounts, establishing game lobbies, and playing a live match directly inside the web browser.

---

#### Pre-check List Before Testing

{{% notice warning %}}
Before opening the browser, confirm all of the following conditions are met. Skipping any item will result in CORS errors or a blank game screen.
{{% /notice %}}

- [ ] Have you redeployed SAM with `CorsOrigin=https://<YOUR_CLOUDFRONT_DOMAIN>` at step 5.5.2?
- [ ] Has the Frontend been built and uploaded to the S3 Bucket? (via CI/CD in 5.6 or manually with `aws s3 sync dist/ s3://<BUCKET>/`)
- [ ] Does navigating to your CloudFront domain in a browser show the game UI (not a blank page)?

---

#### Web UI Verification

1. Open your browser and navigate to the **CloudFrontDomainName** obtained from the Outputs in step 5.5.
2. Click **Sign Up** to register two player accounts.
3. Open **two side-by-side browser windows** and log in with the registered accounts.
4. Join the online matchmaking system by:
   - Clicking **Play vs Player (Ranked)** -> choosing **Join Queue** on both accounts to automatically match and connect into a game room.
   - *(Alternatively, you can choose Private room to manually host and invite players via a room code).*
5. Place your fleet of battleships on the 10x10 grid (you can click **Auto Arrange**) and click **Ready** on both screens to start the match.
6. Take turns clicking coordinates on the grid. Confirm that hits, misses, and game turns sync instantly via WebSocket.
7. Complete the match until a winner is declared so that the system updates the ELO Rank Points.

**Matchmaking and Game Staging flow:**

![Deployment mode select](/images/5-Workshop/5.7-Integration-Test/gameplay-mode-select.png)

![Choose Ranked Battle Mode](/images/5-Workshop/5.7-Integration-Test/gameplay-battle-protocol.png)

![Queue system searching for opponents](/images/5-Workshop/5.7-Integration-Test/gameplay-matchmaking-queue.png)

![Opponent connected and ready lobby](/images/5-Workshop/5.7-Integration-Test/gameplay-opponent-found.png)

![Ship placement and shipyard staging](/images/5-Workshop/5.7-Integration-Test/gameplay-fleet-staging.png)

![Real-time active battle screen](/images/5-Workshop/5.7-Integration-Test/gameplay-fighting.png)

---

#### Verify Resources (AWS Console)

**Match Ranked Results on Web UI:**

![Victory result screen](/images/5-Workshop/5.7-Integration-Test/gameplay-victory.png)

**DynamoDB User Table:**

1. Navigate to **AWS Console → DynamoDB → Tables → User**.
2. Select **Explore table items**.
3. Verify that the winner's ELO/rank points have been successfully incremented in the database.

![ELO rank update in DynamoDB User Table](/images/5-Workshop/5.7-Integration-Test/dynamodb-elo-update.png)
