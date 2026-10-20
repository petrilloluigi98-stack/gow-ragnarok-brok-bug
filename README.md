# God of War Ragnarok - Brok Sigil Arrow Revive Exploit | QA Case Study

**Platform:** PS5 / God of War Ragnarok
**Type:** Gameplay / Systems Interaction Bug - Sigil + Axe = Unintended Revive
**Severity:** High - Breaks Story Logic via Gameplay Exploit
**Role:** QA Tester - Petrillo Luigi

## Bug Summary
When Brok is in dead state (story flag `is_brok_dead=True` or downed at funeral area), Kratos can unintentionally revive him by throwing the Leviathan Axe into stacked Sigil Arrows. The Sigil elemental chain reaction triggers the generic `companion_revive` event, ignoring the story death flag.

## Steps to Reproduce
1. Go to Brok's funeral location at Sverd Sands (Brok is dead on pyre / story dead flag active)
2. Equip Sigil Arrows (purple arrows)
3. Shoot 2-3 Sigil Arrows stacked near Brok's corpse / death location
4. Throw Leviathan Axe (L2 + R2) directly at the stacked Sigils to trigger Frost explosion chain
5. Observe: The Sigil frost explosion triggers `on_elemental_burst` which calls `revive_nearby_companion()`, and Brok stands up alive

## Expected vs Actual
- **Expected:** Sigil + Axe explosion should do nothing to Brok when `is_brok_dead=True`. Revive should be blocked if story flag is dead. Only frost damage / no effect.
- **Actual:** Sigil explosion triggers generic companion revive. Brok revives alive even though he is supposed to be permanently dead. `story_flag_brok_dead=True` is ignored by `sigil_burst_revive_check`.

## Evidence
- Video: Kratos throwing Axe into Sigil Arrows near Brok's body -> Brok revives
- Repro rate: 80% when sigils are stacked x3

## Technical Analysis
Root Cause: 
`sigil_arrow_system.on_frost_burst()` calls `check_revive_in_radius()` which does not filter for `is_story_permadead`. It only checks `is_companion_down`.

Fix: 
In `check_revive_in_radius()`, add condition:
`if companion.has_flag("story_permadead"): return`
Block revive if story death flag is True.

Also add exclusion list: `perma_dead_npcs = [brok]` should never be revivable by elemental triggers.

## Tools Used
- PS5 Gameplay Testing
- Systems Interaction Testing (Sigil Arrows + Leviathan Axe)
- Python log check for flag mismatch

## Skills Demonstrated
QA Gameplay Testing, Systems QA, Edge Case Testing, Bug Reporting, Exploit Testing

---
**Contact:** Luigi Petrillo | QA Tester | Naples, Italy
