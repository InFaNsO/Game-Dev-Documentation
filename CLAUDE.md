# Project: 2026 Gaming Vault

Obsidian vault for YouTube content planning and game design. The flagship work is the design of a "dream game" and its first shippable pillar, the **Looter Shooter** (`Game Development/Main Game/Mechanics/Looter Shooter/`).

This file is the shared, version-controlled brain across machines (home PC + office laptop). It syncs via git. The two sections below are maintained by Claude.

---

## ⚠️ Setup Pending — Re-add SessionStart hook after proper sync

> **Note to Claude:** The auto-sync `SessionStart` hook was intentionally REMOVED. The authoritative project info lives on another PC and has not yet been synced here. Do NOT reconcile memory from this incomplete local copy.
>
> **Once the other system's work has been properly synced into this repo** (pulled and confirmed up to date), re-add the hook so session-start memory reconciliation resumes. Create `.claude/settings.json` (committed — NOT `settings.local.json`) with:
>
> ```json
> {
>   "hooks": {
>     "SessionStart": [
>       {
>         "hooks": [
>           {
>             "type": "command",
>             "command": "git log --oneline -n 15",
>             "timeout": 15,
>             "statusMessage": "Loading recent commits for CLAUDE.md memory reconciliation..."
>           }
>         ]
>       }
>     ]
>   }
> }
> ```
>
> Then delete this "Setup Pending" section.

---

## Current Plan of Action

> Next few goals we're working toward. Claude keeps this current — update/check off as work lands.

- [ ] _(no active goals captured yet — populate at next planning session)_

---

## Claude Memory

> Curated, human-readable project context plus a digest of recent changes. `git log` is the authoritative change record; this section is the fast cross-machine handoff summary. Claude reconciles this at session start (a `SessionStart` hook surfaces recent commits) and whenever a meaningful change lands. **After Claude updates this file, commit & push so the other machine sees it.**

### Project context

- **Development strategy — ship one pillar first, then carry forward.** The dream game is a four-pillar fusion: roguelike storytelling + city builder + automation + looter-shooter/dungeon-crawler (`Main Game/01 Concept & Idea.md`, `04 Game Pitch.md`). Plan: build the hardest, most self-contained pillar — the Looter Shooter — as its own polished standalone game first, then fold its tech into the main game with **additive changes only, no rewrites** (`Looter Shooter/10 Unity Code Architecture.md` §22 Carry-Forward Checklist). Rationale: procgen room assembly + the Sparks of Hope × XCOM × Expedition 33 tactical combat model are the riskiest unknowns; the standalone de-risks them and ships a real product.
- **Scope firewall.** City-building, automation, party combat, other biomes (Genesis Vats / Prism Forge), and the endgame Choice are explicitly scope-cut from the prototype (`Looter Shooter/08 Standalone Prototype - Design.md` §10). Don't pull them into prototype work. Features are tagged MVP / V1 / Stretch (`09 Complete Feature List.md`) — honor that prioritization.
- **Tech stack.** Unity 6 + C#, single-player, single project. Data-driven (ScriptableObjects) + decoupled (service locator, typed event bus, state machines). Likely solo/very small indie; no multiplayer.
- **Looter-shooter doc structure.** Numbered files 00–15 in `Looter Shooter/`. `00 Looter Shooter.md` is the hub. Lore: 11 Factions, 12 Akashic/Bleed, 13 Campaign, 14 Naming Glossary, 15 Grinder Trust Arc. Implementation: 08 Prototype Design, 09 Feature List, 10 Unity Architecture.
- **"Topic N" numbering.** The user refers to planning sessions by "Topic N" (a session sequence, NOT filenames). Topic 6 = Grinder Trust Arc (commit `99da80a`). As of 2026-05-29, topics 7–10 (for "finalizing the story") had no surviving record in any tracked file — capture them here if the user clarifies.

### Recent changes (digest)

- **2026-05-29** — Set up cross-machine workflow: added `.gitignore` (ignores only `.claude/settings.local.json`) and created this `CLAUDE.md` as the shared brain. The `SessionStart` auto-sync hook was prepared then **removed** pending a proper sync from the other PC — see the "Setup Pending" section above for re-adding it. Earlier in repo history: looter-shooter lore/naming/factions/prototype docs and the Grinder Trust Arc (Topic 6) were committed.
