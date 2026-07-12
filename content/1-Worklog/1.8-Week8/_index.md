---
title: "Week 8 Worklog"
date: 2026-06-28
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Week 8 Objectives:

* Improve and redecorate the interface of the entire project to make it look better, more vibrant, and professional.
* Create images for 6 new warships to give players more options when setting up their fleet.
* Integrate complete game audio: sound effects for miss, hit, ship explosion, along with winning/losing themes, lobby music, and battle music.
* Create a Settings panel to let players customize volume levels or toggle background music and game sound effects.
* Redesign the PvP battle board to show more player details and implement an in-game chat feature for players to talk during matches.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| 2 | - Redecorate CSS layouts for homepage, lobby, and battle screens <br> - Edit the entire project's interface, updating buttons, borders, and backgrounds to make the app look much better | 06/22/2026 | 06/22/2026 | https://cloudjourney.awsstudygroup.com/ |
| 3 | - Create and size assets for 6 new warships to fit the game grid <br> - Create images for 6 new warships; add these ship images to the player's selection list before matches | 06/23/2026 | 06/23/2026 | https://cloudjourney.awsstudygroup.com/ |
| 4 | - Configure game audio controls using helper libraries (such as Howler.js) <br> - Write `soundService.js` to integrate background music for the lobby (`menuMusic`) and battle phase (`battleMusic`) via `syncBackgroundMusic`; add sound effects for miss, hit, and ship explosion using `playSound` | 06/24/2026 | 06/24/2026 | https://cloudjourney.awsstudygroup.com/ |
| 5 | - Construct volume adjustment sliders and audio toggles in React <br> - Create the Settings feature; build volume sliders for adjusting background music and game sound effects using `setSoundSettings` and `getSoundSettings` | 06/25/2026 | 06/25/2026 | https://cloudjourney.awsstudygroup.com/ |
| 6 | - Optimize player information display layouts on the PvP combat screen <br> - Redesign the PvP battle room, adding areas to show detailed info for both sides (name, rank tier, avatar, wins/losses) | 06/26/2026 | 06/26/2026 | https://cloudjourney.awsstudygroup.com/ |
| 7 | - Implement real-time chat functionality via socket connections <br> - Integrate `PvpBattlePanels.jsx` chat component and implement in-game chat in the PvP room by sending socket signals (`sendPvpSignal`) and updating the chat logs (`appendChatMessage`); test the whole system | 06/27/2026 | 06/28/2026 | https://cloudjourney.awsstudygroup.com/ |

### Week 8 Achievements:

* **Improved Project Interface:**
  * The entire homepage, lobby, and battle room interface were updated to look much cleaner and more professional.

* **6 New Warships Added:**
  * Created images and successfully imported 6 new types of warships, giving players more options to place.

* **Complete Audio System:**
  * Successfully integrated BGM for the lobby, battles (`syncBackgroundMusic`), wins, and losses.
  * Added sound effects for shots hit, shots missed, and ship explosions (`playSound`).

* **Settings Panel for Sound Customization:**
  * Finished the Settings feature, letting users easily adjust volume or toggle background music and sound effects with sliders (`setSoundSettings`, `getSoundSettings`).

* **Upgraded PvP Interface & Chat Feature:**
  * The PvP screen now displays complete statistics for both competitors (avatar, name, rank).
  * Added a chat feature (`PvpBattlePanels.jsx`) to let two players message each other directly while playing using socket signal (`sendPvpSignal`) and updating the chat screen (`appendChatMessage`).
