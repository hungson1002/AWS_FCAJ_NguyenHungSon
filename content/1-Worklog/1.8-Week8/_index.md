---
title: "Week 8 Worklog"
date: 2026-06-27
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Week 8 Objectives:

* Research and implement the Custom Shipyard feature, allowing players to design their own ships.
* Develop a leaderboard system to display the TOP 5 players with the highest scores across all rank tiers.
* Optimize the user interface (UI) for both Desktop and responsive mobile views.
* Enhance system security and stability by configuring Throttling (Rate Limiting) on API Gateway.

### Tasks performed during the week:

| Day | Task | Start Date | Completion Date |
| --- | ---- | ---------- | --------------- |
| 2   | - Researched Custom Shipyard solutions and grid coordinate systems to allow users to draw custom ship shapes <br> - Defined validation rules for custom-drawn ships | 06/22/2026 | 06/22/2026 |
| 3   | - Implemented the Custom Shipyard UI on the Frontend with an interactive drawing grid <br> - Built validation algorithms (ensuring contiguous blocks and correct cell counts) and saved custom ship configurations | 06/23/2026 | 06/23/2026 |
| 4   | - Designed DynamoDB schemas and developed Lambda Functions to query leaderboard statistics <br> - Retrieved TOP 5 player data with the highest scores for all rank tiers | 06/24/2026 | 06/24/2026 |
| 5   | - Integrated the Leaderboard UI on the Frontend to show detailed rankings <br> - Optimized the user interface (UI) for Desktop and completed responsive adjustments for mobile and tablet screens | 06/25/2026 | 06/25/2026 |
| 6   | - Configured Throttling (Rate Limiting) on AWS API Gateway endpoints <br> - Set limits on request rates and burst sizes to prevent API abuse and spamming | 06/26/2026 | 06/26/2026 |
| 7   | - Conducted comprehensive testing of all new features (Custom Shipyard, Leaderboard, API Throttling) <br> - Fixed UI bugs on responsive views and compiled the Week 8 progress report | 06/27/2026 | 06/27/2026 |

### Achievements:

* **Completed the Custom Shipyard feature:**
  * Enabled players to freely draw and customize their ship layouts based on personal preferences instead of using static templates.
  * Successfully integrated placement validation (ensuring contiguous blocks and correct sizes) before the matchmaking starts.

* **Implemented the Leaderboard system:**
  * Built APIs to query DynamoDB and retrieve the TOP 5 players with the highest scores across all rank tiers (Bronze, Silver, Gold, Platinum, Diamond, etc.).
  * Designed an intuitive, clean leaderboard interface showing rank badges and detailed win statistics.

* **Enhanced User Interface and Responsiveness:**
  * Optimized layout structures for Desktop screens, increasing sizes for primary interaction areas and controls.
  * Resolved layout clipping, misalignments, and rendering issues on mobile and tablet screens.

* **Secured API endpoints with API Gateway Throttling:**
  * Successfully configured Throttling (Rate and Burst limits) on AWS API Gateway.
  * Protected server resources from API spamming, ensuring system stability and reducing unwanted AWS Lambda invocation costs.
