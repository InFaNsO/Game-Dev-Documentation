# City Builder — Logistics & Transport (Phase 3)

> The Main↔Satellite spine — *the* system this game exists to prove for the dream game. The 3-tier ladder turned into moving-parts gameplay. Forks A–D locked by user 2026-06-07; numbers TBD.

---

## The progression spine (LOCKED)

`grid-roads → boats (intra-map) → Vayu-freight (inter-map) → Akash portals (instant)`. Each tier **widens the distance-scope you can serve** and **cheapens crossing it.** Doubles as the **era ladder** (Phase 4 hangs off this).

## 3.1 Intra-island distribution (LOCKED — fork A: cozy Anno)

**Anno-style warehouse/market radius + grid roads + carts.** Buildings inside a warehouse's radius are served; roads connect them. **No road-hauler micro.** Readable, cheap, cozy.

## 3.2 Inter-island & inter-map transport (LOCKED — fork B: abstracted routes)

**Abstracted trade routes** — assign carriers to a route, they auto-ferry; throughput = carriers × capacity. **NOT** simulated traffic agents. **Inter-map = the Anno "session-transfer" model:** carriers pathfind to the **map edge** (the transfer point) and cross between biome-maps (sail off-edge → arrive in the other map). Matches the biome-as-map structure.

## 3.3 Freight fuel = Vayu Mani (LOCKED — fork C: fuel, not free)

Refined **Vayu = the freight network's fuel / throughput**, **Avian-run.** More Vayu → more/faster carriers. Logistics is a **Vayu sink** → makes the Vayu biome strategically essential and **ties shipping into the Mani economy** (shipping isn't free — a Vayu shortfall throttles the whole network).

## 3.4 Route-management depth = SIMPLE (LOCKED — fork D)

**Simple per-route management (low micro)**, NOT Cities:Skylines traffic-tending depth. Rationale: route **count will be high** (every Mani × every good × every colony) → per-route micro would drown the player. **The challenge lives at the NETWORK scale** — balancing many routes so supply meets demand across a multi-biome, multi-species portfolio — *not* tending individual routes.
- **Implication:** invest in strong **route-management UI / QoL** (route list, per-good supply/demand overview, shortage alerts) since volume is the design reality.

## Storage & bottlenecks

Ports + warehouses with capacity caps. The macro challenge = **portfolio balancing** (keep every colony fed; no starvation) — the Anno "logistics maintenance" fantasy, at network scale.

## Akash capstone (LOCKED direction)

Space-fold **portals = instant transfer** → effectively **removes route management at endgame.** The logistics endgame = *conquering distance itself* — especially satisfying given the high route volume D creates.

> ⚠️ **OPEN — DEFERRED TO DEVELOPMENT (flagged 2026-06-13).** Tension to resolve at build time: instant portals *delete the logistics core loop they're meant to cap.* Two candidate framings, undecided — **do not act on this until we reach this part of development:**
> - **(a) Narrative-only capstone** — the portal/Akash Synthesis is a cinematic great-work that completes the game, not a playable mechanic, so it never trivializes live logistics.
> - **(b) Playable reward (gated)** — portals are a real mechanic the player earns, but hard-gated (rare Akash materials, long build, throughput caps / breakable) so endgame logistics keep meaningful tension.
> Decide once the logistics game is playable and we can feel whether the late-game goes hollow. Until then, leave the locked "instant transfer" direction as-is.

## 3.5 Threat interaction — NONE (LOCKED 2026-06-07)

**Routes are not attacked.** Tower-defense is reserved for the dream game (see `04 World & Setting`), so nothing severs routes or besieges ports. Logistics tension is **purely economic** (supply/demand balancing across the network). *(Environmental Mani-weather as a soft route-disruptor = deferred, revisit post-prototype.)*

---

> **Next:** Phase 4 — Era progression (hangs off the spine above), Phase 5 — depth allocation + bounding line. (Phase = threat/TD resolution still open.)
