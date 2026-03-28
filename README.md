# RB Match Buddy
### Overview

**Riftbound Match Buddy** is a real-time synchronization tool designed to bridge the communication gap between Table Judges and Backstage Staff (Head Judges or Coverage Teams). It provides a shared digital state for resource tracking and rules enforcement, optimized for Feature Match environments.

---

## 1. Operational Roles & Control Logic

The system uses a **Room Code** architecture to connect devices via Firebase with two distinct permission levels:

* **Judge (Admin)**: The official at the table. Has full authority to modify stats, manage timers, and log penalties.
* **Viewer (Backstage/HJ)**: Provides a real-time, read-only mirror of the match.
    * **Stat Flagging**: Viewers can tap any resource card to "flag" it (turning it red on the Judge's screen). This allows backstage officials to discreetly signal a potential error (e.g., a missed draw or incorrect energy count) without verbal interruption.

---

## 2. Judging Utilities

### The "My Turn" Logic & Automation
To ensure the log and resources are accurate, the Judge must trigger the **"My Turn"** button for the player starting their turn. This action performs several automated tasks:
* **Resource Reset**: Clears the current *Energy* and *Power* for both players.
* **Rune Generation**: Automatically adds **+2 Runes** to the active player (capped at 12).
* **Turn Counter**: Increments the turn count for the active player.
* **Global Sync**: Updates the turn status for all Viewers and logs the event with a timestamp.

### Scoring Dynamics
The **Points** container is enhanced with specific shortcuts:
* **Standard (+/-)**: For generic adjustment.
* **Conquer (+C)**: Specifically for points gained through *Conquer*.
* **Hold (+H)**: Specifically for points gained through *Hold*.
* *Note: All scoring types are explicitly labeled in the match history log for post-match audit.*

### Judge Call & Ownership 
The **Judge Call** is a global alert system that triggers a pulsing red overlay on all connected devices.
* **Closing Ownership**: To maintain a clear chain of command, **the call must be closed by the same role that opened it**. If a Viewer (Backstage) opens a call, they are the only ones who can dismiss the red glow, signaling that the backstage concern has been resolved.

### Auditing & Logs
Every interaction—from a single point change to a "Judge Call"—is recorded in a **Match History**.
* **Transparency**: Both the Judge and Viewers share the same log.
* **Post-Match Report**: The entire log can be exported as a `.txt` file, providing a second-by-second account of the match for late-round discussions or coverage recaps.
---

## 3. Infractions & Penalties 

The tool includes a professional **Penalty Manager** aligned with official standards (GamePlay Errors, Tournament Errors, Unsporting Conduct). Unlike match stats, **resetting the game does NOT clear Infractions**.
This ensures that even if a match is reset for a new Game 2 or 3, the Judge maintains a persistent record of Warnings or Game Losses assigned during the entire match session.
* **Subtype Database**: Includes pre-configured subtypes like *Missed Trigger, Decklist Error, Slowplay, etc.*

---

## 4. Time Management & Safety

### Extra Time Logic
* **Supplementary Tracking**: This timer tracks ruling time or technical pauses.
* **PurpleFox Alignment**: This value is **independent**. It should be treated as a "Delta" to be added to the official extra time already recorded in primary tournament software (like PurpleFox).

### Navigation Guard
To prevent accidental data loss, the tool implements a **History API interceptor**. Standard mobile navigation (back-swipes or hardware back buttons) is blocked and redirected to a custom confirmation modal, protecting the active session.

---

## 5. Technical Notes & License

* **Undo Engine**: A 30-step deep undo stack allows the Judge to revert any accidental input instantly.
   * The Undo engine also reverts global flags and ownership markers. *If a Viewer triggers an accidental Judge Call*, the Judge (admin) can use Undo to revert the match to a "Pre-Call" state, effectively erasing the accidental alert and its corresponding log entry.*
* **Wake Lock**: Integrated to prevent mobile screens from dimming during long 40+ minute rounds.
* **License**: **Free to Use**. Provided as a community resource for the Riftbound ecosystem. You are free to host, use, and modify this tool for any local or major competitive event.

---

**Author**: [Atled]  
**Architecture**: Firebase Real-time Firestore.
