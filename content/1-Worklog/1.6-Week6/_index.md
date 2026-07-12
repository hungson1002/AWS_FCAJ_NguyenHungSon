---
title: "Week 6 Worklog"
date: 2026-06-14
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Week 6 Objectives:

* Research and configure AWS Cognito to establish a connection with the project, design the login and registration interfaces linked to Cognito to implement this authentication feature.
* Set up necessary configurations in Cognito to support signing in with Google and Facebook.
* Use AWS Lambda and Amazon API Gateway to build a serverless backend.
* Set up Amazon DynamoDB database to store user information.
* Create images of the first 4 warships to add to the Player vs AI mode.
* Test and fix issues with ship rotation, ship placement, and dragging and dropping ships on the board.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| 2 | - Configure AWS Cognito (User Pools) for user management <br> - Create and configure a Cognito User Pool; design Registration (`Register.jsx`) and Login (`Login.jsx`) interfaces; install the Amplify SDK to connect interfaces with Cognito and call authentication functions (`registerUser`, `confirmRegister`, `loginUser`) | 06/08/2026 | 06/08/2026 | https://cloudjourney.awsstudygroup.com/ |
| 3 | - Integrate Google and Facebook login on Cognito <br> - Set up App Registrations on Google Cloud Console and Facebook Developers; configure Cognito Identity Providers and call `loginWithSocialProvider` to enable Google and Facebook login | 06/09/2026 | 06/09/2026 | https://cloudjourney.awsstudygroup.com/ |
| 4 | - Implement serverless backend using AWS Lambda and API Gateway <br> - Create a REST API on API Gateway; write and configure Lambda function to retrieve player profile details (`getUser.js`) | 06/10/2026 | 06/10/2026 | https://cloudjourney.awsstudygroup.com/ |
| 5 | - Configure DynamoDB table to save and manage user data <br> - Create a DynamoDB table for player profiles; write a Lambda function (`createUser.js`) connected to DynamoDB to automatically save user details right after successful signup | 06/11/2026 | 06/11/2026 | https://cloudjourney.awsstudygroup.com/ |
| 6 | - Prepare and optimize images of warships for the game <br> - Create images for the first 4 warships; add these images to the code and display them on the Player vs AI board | 06/12/2026 | 06/12/2026 | https://cloudjourney.awsstudygroup.com/ |
| 7 | - Implement grid coordinates and drag-and-drop mechanics for warships <br> - Test and fix issues with ship rotation (`rotateShip` / `handleRotate`), dragging and dropping (`dragStart`, `dragOver`, `drop`), and placing ships onto the grid (`isValidPlacement`) to make board setup smoother | 06/13/2026 | 06/14/2026 | https://cloudjourney.awsstudygroup.com/ |

### Week 6 Achievements:

* **Successful User Authentication Integration (AWS Cognito):**
  * Configured Cognito User Pool and connected it to the Frontend.
  * Designed the Registration (`Register.jsx`) and Login (`Login.jsx`) interfaces, integrating them with Amplify API functions (`registerUser`, `confirmRegister`, `loginUser`).
  * Enabled user login with regular accounts and social login via Google and Facebook (`loginWithSocialProvider`).

* **Serverless Backend API Architecture:**
  * Completed the API Gateway REST endpoints and integrated them with AWS Lambda (`getUser.js`) to handle user profile retrieval requests.

* **Database Integration with DynamoDB:**
  * Created player profile tables in DynamoDB.
  * Finished Lambda function (`createUser.js`) running as a Post Confirmation Trigger to automatically record user details in the database upon successful registration or social sign-in.

* **Create and Add Warship Images:**
  * Created and successfully imported the images of the first 4 warships.
  * Rendered these warship images on the Player vs AI mode board.

* **Fix Ship Placement and Rotation on the Board:**
  * Fixed ship rotation bugs (rotating horizontally/vertically no longer goes off the grid or overlays other ships).
  * Fixed drag-and-drop and ship placement issues on the grid to make it easier for players to set up their board before matches start.
