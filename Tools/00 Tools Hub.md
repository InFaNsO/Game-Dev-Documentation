# Tools Hub

> The custom + third-party tooling behind the portfolio's content pipeline. Design/lore lives under `Game Development/`; this section documents **how assets get made**. Last updated 2026-06-14.

> **⚠ Status note (2026-07-31): Kimodo is deprecated for Last Rite combat animation.** Output quality was judged unacceptable for combat; Last Rite combat clips now come from a purchased pack (**Mega Animation Pack v1.8**, Unity Asset Store) retargeted via Unity Humanoid — see [[1h Last Rite - Player Moveset & Animation Plan]] + [[1f Last Rite - Combat Iteration Log]]. The Kimodo pipeline below is **retained for reference/other uses** (non-combat motion, experiments, future games) — nothing is deleted.

## What's here

- **[[01 AssetForge]]** — our in-house Blender asset pipeline (mesh → retopo → rig → animate → export), driven over MCP. The hub tool; everything else plugs into it.
- **[[02 Prompting & Motion Quality]]** — how to write prompts + tune settings to get high-quality motion out of Kimodo (and how the retarget step affects the result).

## The pipeline at a glance

```
Concept (Gemini) → Mesh + texture (Meshy) → Retopo/LOD (Meshy) → Rig (Meshy, Mixamo skel)
   → Motion (Kimodo, text→motion) → Retarget onto rig (AssetForge) → Export FBX/GLB (Unity)
```

| Tool | Role | Where it runs |
|---|---|---|
| **AssetForge** | Orchestrates the whole pipeline; exposes it as MCP tools; does the Kimodo→Blender retarget | Blender addon + a thin MCP server. Repo: `InFaNsO/AssetForge`, submodule at `tools/assetforge` |
| **blender-mcp** | Lets Claude drive Blender (execute code, screenshots) | `blender-mcp.exe`, talks to the Blender addon on port 9876 |
| **Meshy** | AI mesh generation, retopology/LOD, auto-rigging (consistent 24-bone Mixamo skeleton) | Cloud API (credits) |
| **Kimodo** (NVIDIA) | Text→motion diffusion (SOMA skeleton, 77 joints) | **Local** GPU server (free) — Modal cloud as fallback. See [[01 AssetForge]] |
| **Modal** | Cloud GPU host for Kimodo (fallback only now that local works) | A10G, ~$0.02/clip batched |

## Conventions

- **GLB is the asset interchange** (carries PBR, skeleton, anims, `extras`). FBX only at the Unity export boundary.
- **One rig discipline** — Meshy auto-rig produces a consistent Mixamo skeleton; SOMA→Mixamo bone map lives in AssetForge.
- **API keys live in the Blender addon preferences** — never in chat, the repo, or git.
- Spend is tracked in [[Production Cost Ledger]].

See also the project brain at the repo root (`CLAUDE.md`) and the asset-creation workflow in Claude's memory.
