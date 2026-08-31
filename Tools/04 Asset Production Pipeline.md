# 04 — Asset Production Pipeline (Studio SOP)

> **Scope**: end-to-end production of game-ready 3D assets for Last Rite (Unity 6, URP, Humanoid).
> **Companion**: the executable version of this pipeline is the `asset-pipeline` skill (`.qoder/skills/asset-pipeline/SKILL.md`) driven by the agent team in `.qoder/agents/`.
> **Companion docs**: `01 AssetForge.md`, `02 Prompting & Motion Quality.md`, `03 SOMA-to-Meshy Retarget Map.md`, `1b Last Rite - Art Bible.md`, `Production Cost Ledger.md`.

---

## 1. Philosophy

One command in, one approved asset out. A single **Pipeline Director** (art-director) classifies every brief and routes it to one of four specialized pipelines built on the same 10-stage canon. No work reaches the owner for approval until it has passed the **Quality Auditor** (compliance) and the **Art Director** (creative gate).

```
Brief → Stage 0 classification → specialized pipeline → gates → shipped asset

Gate doctrine:  specialist → QUALITY AUDITOR → ART DIRECTOR → OWNER approval
                             (compliance)      (creative)      (final)
```

- Failed audits produce **mandatory rework orders** back to the responsible expert — flawed work never escalates upward.
- Every reworked deliverable gets a full fresh re-audit.
- The auditor is accountable: any defect found downstream in passed work is logged as an Audit Failure Incident with a corrected checklist.

## 2. Team Roster

| Role | Agent | Stages owned |
|---|---|---|
| Pipeline Director / Art Director | `art-director` | Stage 0 + all creative gates |
| Quality Auditor | `quality-auditor` | Every deliverable, pre-gate |
| Concept Artist | `concept-artist` | 1–3 |
| Asset Analyst | `asset-analyst` | 4–5 |
| Blender Modeler | `blender-modeler` | 6, 8, 9 |
| Texture Artist | `texture-artist` | 7 (+ VFX sprite sheets) |
| Unity Integrator | `unity-integrator` | 10 |

## 3. Stage 0 — Classification

| Asset type | Signals | Pipeline |
|---|---|---|
| Character / creature | animates, fights, boss/player/NPC | **A** |
| Environment | spaces, arenas, strata, architecture | **B** |
| Prop | standalone object | **C** |
| VFX | particles, sparks, trails | **D** |
| Mixed | "boss arena" | decomposed into parallel sub-jobs |

Classification is confirmed with the owner before routing.

## 4. The 10-Stage Canon (roles, tools, QC)

| # | Stage | Expert | Primary tools | QC checkpoint |
|---|---|---|---|---|
| 1 | Concept brainstorming | Concept Artist (+ Director) | Art Bible, lore docs, chat | Narrative fit, silhouette idea, 2–3 variants ⛔ owner |
| 2 | Concept art generation | Concept Artist | Gemini (Chrome MCP) / SocialFoundry Image Studio + `.prompt.json` sidecars | Variants presented, reproducible prompts ⛔ owner picks |
| 3 | Refinement + model list | Concept Artist | Reference-sheet conventions + SAM segmentation (SocialFoundry mask/matte) for component isolation | Full detail checklist resolved; **validated, segregated model list** with per-item component cuts ⛔ audit + director |
| 4 | Asset decomposition | Asset Analyst | Model list, budget rules | Sub-asset table, budgets, VFX excluded ⛔ audit + director |
| 5 | Modular analysis | Asset Analyst | Kit/reuse methodology | Kits, reuse matrix, procedural variation plan ⛔ audit + director |
| 6 | High-poly modeling | Blender Modeler | Blender (bpy scripting), AssetForge/Meshy | Silhouette-first, watertight where required ⛔ audit + director |
| 7 | UV & texturing | Texture Artist | Blender UV + 4-map PBR set | Texel density, map completeness, palette governance ⛔ audit + director |
| 8 | Baking | Blender Modeler | Blender bake (Normal/AO/Curvature) | Clean high/low pairs, no artifacts ⛔ audit |
| 9 | Low-poly optimization | Blender Modeler | Retopo, decimate, LOD tools | Budget adherence (triangle counts), topology ⛔ audit + director |
| 10 | Unity export | Unity Integrator | FBX export, Unity Editor (Unity MCP) | Naming, import, materials, in-editor validation ⛔ audit + director + owner |

## 5. Pipeline A — Character / Creature

1–3 Concept with the **full character checklist**: bone structure, body shape, facial structure + complexion, hair style, clothes itemized (top / bottom / shoes / outerwear), accessories enumerated, scars + minute deformities, weapon. Output includes the segregated model list.
4–5 Manifest with budgets and rig plan.
6 Body = **single watertight mesh** (Meshy auto-rig requirement); hair, clothes, accessories, weapon = **separate meshes** — arms never merged into the torso (clean skinning).
7 4-map PBR + **RGB mask-tint** (R = primary body, G = accent metal, B = trim) for infinite colorways via MaterialPropertyBlock.
8 Bake separate parts. 9 Low-poly: **15–20k quads (≈30–40k tris)** body budget, animation-ready edge loops at joints.
Rig: Meshy auto-rig on the body LOD; parts skinned/socketed onto the shared 24-bone skeleton.
10 Unity: Humanoid avatar, `M_` URP/Lit materials, combat clips from **Mega Animation Pack** (Kimodo = non-combat only).

## 6. Pipeline B — Environment

1–3 Concept with the **full environment checklist**: sub-asset list (walls, floors, pillars, stairs…), props and set dressing, wear-and-tear story (cracks, holes, grime — where and why), **modularity + procedural randomization plan** (e.g., a wall = ONE base shape; skirtings, damage variants, and accessories like photos scattered onto it via geometry nodes).
4–5 Kit manifest: snap metrics, shared materials + atlases, base-shape vs scatter-piece split, collision needs.
6 Kit pieces + scatter pieces modeled. 7 Trim sheets + atlases, consistent texel density. 8 Bake per piece.
9 **Full LOD chains + collision proxies** — environment is where LOD effort pays off (Art Bible: one guardian on screen, LOD effort → environment).
10 Unity: kits imported; scatter baked to placement data; scene assembled.

## 7. Pipeline C — Prop

Lightweight: concept ⛔ → quick decomposition → model (Meshy or Blender) ⛔ → UV + maps, mask-tint ready ⛔ → simple bake/reduce, minimal LODs → Unity ⛔. Variant-friendly by default.

## 8. Pipeline D — VFX

No modeling. Concept produces sprite/flipbook sheet art ⛔; effects built natively in Unity (Particle System + toon-compatible shaders) with an in-engine look gate ⛔. VFX items are listed in the concept model list but **always excluded from modeling**.

## 9. Hard Rules (all pipelines)

- **Silhouette-first** — every design reads at thumbnail size.
- **Watertight** — single watertight mesh for anything passing Meshy auto-rig; deforming parts stay segregated.
- **Budgets** — hero 15–20k quads (≈30–40k tris); always report **triangle** counts (glTF/FBX triangulation doubles quads).
- **Textures** — 4-map PBR set (BaseColor, Normal, MetallicRoughness/ORM, Emission), named `<Asset>_MapName.png`; no baked lighting color in albedo; raw generator textures must be regraded into the governed palette.
- **Rig/animation** — one shared 24-bone Humanoid skeleton; combat animation = Mega Animation Pack.
- **Unity** — `Assets/3D_Assets/<asset>/`, `M_`-prefixed hand-built URP/Lit materials (never FBX-extracted), FBX Y-up / -Z forward / 0.01 armature scale, `<Name>_unity.fbx`.
- **Process** — owner approval before downloading concept art; every image has a `.prompt.json` sidecar; cost tracked in `Production Cost Ledger.md`.

## 10. Artifact Map

| Artifact | Location |
|---|---|
| Concept art + prompts | `games/_concept-art/<asset>/<asset>_turnaround_vN.png` + `.prompt.json` |
| Build manifest | `games/_concept-art/<asset>/<asset>_build-manifest.md` |
| Working outputs | `tools/assetforge/outputs/<asset>/` (+ `textures/`, `anims/`, `concept/`) |
| Unity assets | `games/01-Last-Rite/Assets/3D_Assets/<asset>/` |

## 11. Known Pipeline Gotchas (from production history)

- Rig the **LOD**, not the high-poly (Meshy 300k-face rig cap).
- Meshy upload timeout must be monkeypatched 60s → 300s.
- Meshy rig stage flattens PBR to one texture — restore the 4-map material after.
- Kimodo cold-start: first clips of a cold batch fail — pre-warm and retry.
- BVH export `--inplace` (no root translation); SOMA BVH is in cm → `global_scale=0.01`.
- glTF bone-tail healing required before Blender retargeting.
- Full gotcha list: `.claude/skills/generate-3d-asset/references/assetforge-gotchas.md`.

---
*Version 1.0 — 2026-08-15. Maintained alongside the `asset-pipeline` skill and the `.qoder/agents/` team contracts.*
