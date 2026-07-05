---
title: "Week 7 Worklog"
date: 2026-06-20
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Week 7 Objectives:

* Learn about real-time socket connections to build online two-player mode (PvP), starting with custom rooms (creating and entering room codes).
* Redesign the ship placement map to make it look better, clearer, and easier to use.
* Build a matchmaking system and divide it into two modes: Casual and Ranked.
* Create 7 badges corresponding to 7 rank tiers in the game: Bronze, Silver, Gold, Platinum, Diamond, Master, and Admiral, along with their specific points requirements.
* Add a rank details popup in the player profile to view available ranks and the player's current rank.
* Use the Phaser library to create animations for hit, miss, ship explosion, and win/loss notifications.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date |
| --- | ---- | ---------- | --------------- |
| 2 | - **Research:** Learn how socket connections work for real-time data communication between two players <br> - **Implementation:** Create room management functions on the backend (`createRoom.js`, `joinRoom.js`); write simple custom room features allowing room code generation and entry for testing | 06/15/2026 | 06/15/2026 |
| 3 | - **Research:** Learn how to arrange and color screen elements to redesign the ship placement board <br> - **Implementation:** Rewrite the board setup screen, adjusting colors, grid boxes, and buttons for a cleaner layout | 06/16/2026 | 06/16/2026 |
| 4 | - **Research:** Learn how matchmaking queues work and how to divide rooms into Casual and Ranked modes <br> - **Implementation:** Write backend matchmaking functions (`matchmakeRoom.js`); establish client-side socket connection to queue players into the proper room | 06/17/2026 | 06/17/2026 |
| 5 | - **Research:** Learn how to calculate rank points (RP) and define 7 rank tiers: Bronze (200 RP), Silver (500 RP), Gold (950 RP), Platinum (1500 RP), Diamond (2200 RP), Master (3100 RP), Admiral (4000 RP) <br> - **Implementation:** Create 7 badge images for each rank; write backend calculation functions (`calculateRankDelta`, `buildRankUpdate`) in `rankService.js`; implement rank-up effects | 06/18/2026 | 06/18/2026 |
| 6 | - **Research:** Learn how to display rank information list on the profile interface <br> - **Implementation:** Create a popup in the Profile page (`Profile.jsx`) for players to view the list of 7 ranks, points requirements, and check their own rank | 06/19/2026 | 06/19/2026 |
| 7 | - **Research:** Learn how to draw animations using the Phaser library <br> - **Implementation:** Write Phaser animations in `BattleEffectsLayer.jsx` for actions: hit (`playMiss`), miss (`playHit`), ship sinking/explosion (`playSunkFinisher`), and the rank-up effect (`RankUpScene`) in `RankUpAnimation.jsx` | 06/20/2026 | 06/21/2026 |

### Week 7 Achievements:

* **Two-player Mode (PvP):**
  * Successfully integrated socket connections to let players create rooms (`createRoom.js`) and enter codes (`joinRoom.js`) to play together.
  * Completed the automated matchmaking queue (`matchmakeRoom.js`) divided into Casual and Ranked modes.

* **Redesigned Ship Placement Map:**
  * Remade the ship positioning screen with matching colors, a clearer grid, and easier board setup.

* **Ranking System:**
  * Established 7 rank tiers from Bronze to Admiral based on RP requirements (Bronze: 200, Silver: 500, Gold: 950, Platinum: 1500, Diamond: 2200, Master: 3100, Admiral: 4000).
  * Finished backend rank calculation functions (`calculateRankDelta`, `buildRankUpdate` in `rankService.js`) and rank-up effects (`RankUpScene`).
  * Added a rank information popup on the player Profile page (`Profile.jsx`).

* **Game Animations:**
  * Used Phaser to successfully create animations in `BattleEffectsLayer.jsx` for hit (`playMiss`), miss (`playHit`), and ship explosions (`playSunkFinisher`).
  * Created a win/loss popup that displays at the end of matches.
