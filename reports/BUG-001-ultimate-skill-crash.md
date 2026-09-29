# [BUG-001] Game Crash during Ultimate Skill execution under high network desync

## 📌 Bug Summary
Executing the character's Ultimate Skill while the client is in a network desync state triggers a `NullReferenceException` in `CombatSystem.cs`, causing the game to crash immediately (Exit Code: 0xC0000005).

---

## ⚙️ Environment Details
* **Build Version:** v2.4.1
* **Environment:** Client-Server Multiplayer (US-East-1)
* **Testing Type:** Black-Box / Log Analysis

---

## 🕹️ Steps to Reproduce
1. Launch the game client and connect to a multiplayer match.
2. Engage in combat with an enemy entity ("Boss_Enemy").
3. Simulate/Induce network degradation (High packet loss > 15%).
4. Wait for a position/entity desync warning to trigger.
5. Execute the "Ultimate_Skill" while desynced.

---

## 🎯 Expected vs. Actual Result
* **Expected Result:** The Ultimate Skill should execute safely or fail gracefully (e.g., skill misses or displays a network error message) without crashing the game.
* **Actual Result:** The game encounters an unhandled `NullReferenceException` attempting to resolve `TargetGroup` and crashes to desktop.

---

## 🔍 Log Evidence Excerpt
```text
[2026-09-16 14:20:26.040] [WARNING] [Network]: Desync detected between Client and Server position for "Boss_Enemy"
[2026-09-16 14:20:27.500] [INFO] [Combat]: Player "Hero_01" executed action "Ultimate_Skill"
[2026-09-16 14:20:27.512] [ERROR] [CombatEngine]: NullReferenceException: Object reference not set to an instance of an object
[2026-09-16 14:20:27.513] [ERROR] [CombatEngine]:    at GameEngine.Services.CombatSystem.ApplyAreaDamage(TargetGroup targets) in CombatSystem.cs:line 84