# God of War Ragnarok - Brok Revive Bug | QA Case Study

**Platform:** PS5 / God of War Ragnarok
**Type:** Critical Gameplay Bug - Companion AI Logic
**Role:** QA Tester - Petrillo Luigi

## Bug Summary
After Brok is downed during combat, the revive prompt appears but the interaction fails. Player is stuck in loop, unable to progress, requiring restart from checkpoint.

## Steps to Reproduce
1. Enter combat encounter with Brok as companion (e.g., in Svartalfheim forge area)
2. Let Brok get downed by enemies
3. Approach Brok and press revive button (Circle)
4. Observe: animation starts but cancels immediately

## Expected vs Actual
- **Expected:** Brok should be revived and rejoin combat.
- **Actual:** Revive animation loops / fails. Game state remains "companion down".

## Evidence
- Video capture attached (PS5 Share)
- Python log parser to analyze state flags: `is_companion_down` stuck on True

## Technical Analysis
Root cause hypothesis: State machine not resetting `revive_cooldown` and `companion_state` after interrupt by enemy attack.
Suggested fix: Add interrupt handler to reset revive state.

## Tools Used
- PS5 Game Capture
- Python (log analysis script)
- JIRA-style bug reporting

## Skills Demonstrated
QA Bug Reporting, Regression Testing, Log Analysis, Python Basics, Documentation

---
**Contact:** Luigi Petrillo | LinkedIn: [il tuo link] | Naples, Italy
