
# Unity Code Architecture *(SHARED FOUNDATION — all games)*

> **PROMOTED to the Main Game level (2026-06-06).** This is a **cross-cutting foundation doc, not a Looter-Shooter doc** — the engine architecture & data-driven/decoupled patterns here are **followed by EVERY game on the build ladder** (see [[07 Testbed Build Plan]], Tier-0 foundation). It was *authored* during the Looter Shooter, so the combat/world specifics below are LS examples; the **general patterns** (assemblies, GameBootstrapper + service locator, typed event bus, ScriptableObject data layer, the top-level state machine, build order) are the part that carries forward and grows additively across all games.

> Lore reference stack (LS-specific): [[Mechanics/Looter Shooter/14 Naming Glossary|14 Naming Glossary]] · [[Mechanics/Looter Shooter/12 The Akashic and The Bleed|12 The Akashic and The Bleed]] · [[Mechanics/Looter Shooter/11 Factions and Species|11 Factions and Species]] · [[Mechanics/Looter Shooter/13 Campaign Structure|13 Campaign Structure]] · [[Mechanics/Looter Shooter/15 Grinder Trust Arc|15 Grinder Trust Arc]]

> ⚠️ **STALE — PENDING RE-ARCHITECTURE (2026-06-02).** This document was written for the *old* combat/world model (continuous tactical space, free-form movement dome, XCOM cover, overwatch, procgen room assembly, firearms/ammo/armor, live-timer Mani grenade). **All of that was redesigned in Phases 1–4** — see **[[06 Element in Looter Shooter|06]] "Combat — …" sections** for the authoritative spec. The code structures below (`CoverSystem`, `OverwatchSystem`, continuous `TacticalSpace`, `Gameplay.Procgen`, `AmmoDef`/`ArmorDef`, grenade timer) are **superseded** and will be re-architected when the build starts (task 4). The new shape: **tile grid (~7×7)** · **no cover / no height / no procgen** (authored single-screen arenas, Encounter-Arena transition) · **dual camera** (tilted-3/4 ↔ OTS reaction-cam) · **side-based phases + AP economy (parry-fed)** · **no firearms/armor** · **Mani-is-the-loot economy** (raw → refining minigame → refined fuel → cast/research). Treat this file as a *legacy reference for the data-driven/decoupled patterns*, not the combat design.

Target: **Unity 6 + C# + URP**, single-player, single-project.
Combat model (REVISED): **grid-tactical (South Park: FbW) × intent telegraph (Into the Breach) × reactive parry (Expedition 33) × tactical↔OTS camera (Valkyria Chronicles)** — self-contained authored arenas, dual camera, AP/parry economy.
World structure (REVISED): **hand-authored** explore world + a **pool of hand-authored single-screen tile arenas** (NO procgen in the prototype; procgen → dream-game dungeons later).
Content layer: **lore-driven** — Mani System/economy, Faction System, Mega Structure environments.

Architecture is opinionated toward *data-driven content* (ScriptableObjects), *decoupled systems* (events + service locator), and *clean transitions* between the two gameplay modes (Real-time Exploration ↔ Turn-Based Combat).

---

## 1. Guiding Principles

1. **Data over code.** Every weapon, item, enemy, recipe, loot table, room prefab, ability, **Mani effect, faction profile** is a `ScriptableObject`. Designers tweak in Inspector.
2. **Two clearly separated gameplay modes.** Exploration and Combat are sibling state machines that own their own input, UI, and update loops. Transition is a first-class event.
3. ~~**Procgen is first-class.**~~ **REVISED — procgen is CUT from the prototype.** The explore world and the combat arenas are **hand-authored**. (Procgen moves to the dream-game dungeon system later.)
4. **Lore is mechanical, not flavor.** Mani System and Faction System are full subsystems, not stat tables.
5. **Composition over inheritance.** MonoBehaviours are thin; logic lives in plain C# services and components.
6. **Event-driven.** Systems don't reach into each other — they subscribe to a typed event bus.
7. **Save-safe by construction.** Anything persistent implements `ISaveable`; references content by stable string id.
8. **Built for carry-forward.** Every system in this prototype must transfer into the main game's dungeon system unchanged or with additive layers only.

---

## 2. High-Level Module Map

```
Assembly: Core            → bootstrap, services, events, save, settings
Assembly: Data            → ScriptableObject definitions (no MonoBehaviour deps)
Assembly: Gameplay.Shared → stats, damage pipeline, inventory model, item instances
Assembly: Gameplay.Explore→ player controller, AI patrol, world loot, stealth
Assembly: Gameplay.Combat → grid (TileGrid), facing/flank, side-based turn manager, AP economy, actions, intent telegraphs, parry/dodge, dual-camera rig, status   [REVISED — was: continuous space, cover, overwatch]
Assembly: Gameplay.Arena → authored arena pool, encounter setup, Encounter-Arena transition   [REPLACES Gameplay.Procgen — CUT]
Assembly: Gameplay.Mani→ raw Mani (loot), Effect Table, refining minigame, research minigame, refined Mani, launcher charges, vein/big-ore   [REVISED — grenade timer retired]
Assembly: Gameplay.Faction→ faction definitions, faction AI overlays, reputation
Assembly: Gameplay.Base   → stash, vendor, crafting, loadout
Assembly: UI              → MVP UI controllers, HUD, screens
Assembly: Editor          → custom inspectors, room prefab validators, asset checkers
```

Dependency direction (asmdef-enforced):
`UI → Gameplay.* → Gameplay.Shared → Data → Core`. No upward references. `Gameplay.Mani` and `Gameplay.Faction` are sibling subsystems Gameplay.Combat depends on (combat queries them; they don't depend on combat).

---

## 3. Bootstrap & Services (Pattern: Service Locator + Bootstrapper)

```csharp
public interface IService { }

public static class Services
{
    static readonly Dictionary<Type, IService> _map = new();
    public static void Register<T>(T svc) where T : class, IService => _map[typeof(T)] = svc;
    public static T Get<T>() where T : class, IService => (T)_map[typeof(T)];
}

[DefaultExecutionOrder(-10000)]
public class GameBootstrapper : MonoBehaviour
{
    [SerializeField] GameConfig config;

    void Awake()
    {
        Services.Register<IEventBus>(new EventBus());
        Services.Register<ISaveService>(new JsonSaveService());
        Services.Register<IItemDatabase>(new ItemDatabase(config.ItemCatalog));
        Services.Register<IRoomLibrary>(new RoomLibrary(config.RoomCatalog));
        Services.Register<ILevelAssembler>(new LevelAssembler());
        Services.Register<IEncounterDirector>(new EncounterDirector());
        Services.Register<ILootDirector>(new LootDirector());
        Services.Register<IManiSystem>(new ManiSystem(config.ManiEffectTable));
        Services.Register<IFactionRegistry>(new FactionRegistry(config.Factions));
        Services.Register<IGameStateMachine>(new GameStateMachine());
        Services.Get<IGameStateMachine>().ChangeTo<BootState>();
    }
}
```

---

## 4. Top-Level Game State Machine (Pattern: State)

```
BootState → MainMenuState → BaseState → RaidLoadingState → ExploreState
                                                ↑              ↓ (encounter trigger)
                                          RaidResultState ← CombatState
                                                ↑              ↓ (victory/flee)
                                                └── ExploreState
```

`RaidLoadingState` runs procgen: assembler stitches rooms, encounter director places Grinder squads with faction-aware AI overlay, loot director places loot + Mani shard spawns at vein anchors, navmesh bakes async.

---

## 5. Event Bus (Pattern: Pub/Sub, immutable struct events)

```csharp
public interface IEventBus : IService
{
    void Publish<T>(T evt);
    void Subscribe<T>(Action<T> handler);
    void Unsubscribe<T>(Action<T> handler);
}

// Core combat
public readonly struct EncounterTriggered { public readonly EnemyActor Source; public readonly List<EnemyActor> Squad; }
public readonly struct CombatEnded       { public readonly CombatResult Result; }
public readonly struct TurnStarted       { public readonly Combatant Actor; }
public readonly struct IntentDeclared    { public readonly Combatant Enemy; public readonly IntentDeclaration Intent; }
public readonly struct ParryResolved     { public readonly ParryGrade Grade; }
public readonly struct StatusApplied     { public readonly Combatant Target; public readonly StatusEffect Effect; }

// Mani-specific
public readonly struct ManiShardPickedUp  { public readonly ManiShardInstance Shard; public readonly Combatant Holder; }
public readonly struct ManiGrenadeArmed   { public readonly ManiShardInstance Shard; public readonly float TimerSeconds; }
public readonly struct ManiGrenadeThrown  { public readonly ManiShardInstance Shard; public readonly Vector3 Target; }
public readonly struct ManiGrenadeDetonated { public readonly Vector3 Position; public readonly ManiEffectDef RolledEffect; }
public readonly struct ManiVeinBroken     { public readonly Vector3 Position; public readonly ManiEffectDef RolledEffect; }

// Faction
public readonly struct FactionReputationChanged { public readonly string FactionId; public readonly int Delta; }

// Procgen
public readonly struct LevelAssembled { public readonly AssembledLevel Level; }

// Inventory / loot
public readonly struct ItemPickedUp { public readonly ItemInstance Item; }
public readonly struct PlayerDied   { public readonly DamageInfo Killer; }
```

---

## 6. Data Layer — ScriptableObjects

All content authored as SOs in `Assets/Data/...`, loaded via Addressables.

```csharp
public abstract class ItemDef : ScriptableObject
{
    public string ItemId;
    public string DisplayName;
    public Sprite Icon;
    public Vector2Int GridSize;
    public float Weight;
    public Rarity Rarity;
    public int BaseValue;
}

public class WeaponDef : ItemDef
{
    public WeaponClass Class;
    public CaliberDef Caliber;
    public int BaseDamage;
    public int ActionCost;
    public int MagSize;
    public float Range;
    public AnimationCurve RecoilCurve;
    public List<ModSlotDef> ModSlots;
    public List<AbilityDef> SignatureAbilities;
}

public class AbilityDef : ScriptableObject
{
    public string AbilityId;
    public int ActionCost;
    public int Cooldown;
    public TargetingMode Targeting;     // Self / Single / AOE / Line
    public List<EffectDef> Effects;
}

public class StatusEffectDef : ScriptableObject
{
    public string StatusId;
    public int DurationTurns;
    public List<EffectStep> PerTurnSteps;
    public bool BlocksMovement;
    public bool BlocksAction;
}

public class AmmoDef : ItemDef
{
    public CaliberDef Caliber;
    public int Penetration;
    public float DamageMul;
    public StatusEffectDef ProcStatus;  // FMJ = none; Mani-infused = roll from Mani Effect Table
    public float ProcChance;
    public bool RollsFromManiTable;  // true for Mani-infused ammo
}

public class ArmorDef    : ItemDef { public int ArmorClass; public ArmorSlot Slot; public List<HitZone> Covers; }
public class ConsumableDef : ItemDef { public List<EffectDef> Effects; }
public class RecipeDef   : ScriptableObject { public List<ItemQty> Inputs, Outputs; public int CraftTime; }
public class LootTableDef: ScriptableObject { public List<WeightedDrop> Drops; }

// Mani-specific data
public class ManiEffectDef : ScriptableObject
{
    public string EffectId;            // "Burn" / "Freeze" / "Phase" / "Summon" / ...
    public ManiTier Tier;            // Common / Uncommon / Rare
    public StatusEffectDef Status;      // optional — many effects just apply status
    public List<EffectDef> CustomEffects; // for "Phase caster" / "Summon Mani-construct" etc.
    public float Radius;
    public GameObject VfxPrefab;
}

public class ManiEffectTable : ScriptableObject
{
    public List<WeightedManiEffect> CommonRolls;
    public List<WeightedManiEffect> UncommonRolls;
    public List<WeightedManiEffect> RareRolls;
    public AnimationCurve TierWeightCurve;  // raw vs refined biases roll distribution
}

public class ManiShardDef : ItemDef
{
    public float DetonationTimerSeconds; // ~5–8s
    public ManiShardKind Kind;        // Raw (random roll) / Refined (chosen, main-game only)
    public ManiEffectDef RefinedEffect; // null for Raw
}

// Faction-specific data
public class FactionDef : ScriptableObject
{
    public string FactionId;             // "Grinders" / "RivalScavengers" / "RogueGrinder" / "AmphibianHusks" / "ReptileHusks"
    public string DisplayName;
    public AIOverlayProfile AIOverlay;   // pack-tribe behavior weights
    public List<EnemyDef> Archetypes;
    public List<WeaponDef> WeaponPalette;
    public LootTableDef FactionLoot;
    public ReputationCurve ReputationTrack;
    public bool IsManiIgnorant;        // true for Grinders — random Mani effects affect them too
}

public class EnemyDef : ScriptableObject
{
    public StatBlock Stats;
    public List<AbilityDef> Abilities;
    public LootTableDef Loot;
    public AIGoalProfile AI;
    public List<IntentTemplate> IntentTemplates;
    public FactionDef Faction;            // faction overlay applied on top of AI profile
}

public class MapDef : ScriptableObject
{
    public string SeedName;
    public AssemblerRuleSet Rules;
    public List<string> AllowedSubThemes; // "MiningTunnel" / "DrillRoom" / "OutdoorCamp" / "ManiVeinChamber"
    public DifficultyCurve Difficulty;
    public Vector2Int SizeRange;
    public float ManiVeinDensityCurve;
}
```

**Flyweight pattern:** `ItemDef` / `ManiEffectDef` / `FactionDef` shared; runtime uses lightweight instances.

```csharp
public class ItemInstance
{
    public string InstanceId;       // guid
    public string ItemId;           // references ItemDef.ItemId (save-safe string)
    public float Condition;
    public int StackCount;
    public List<ItemInstance> Mods;
    public Dictionary<string, object> RuntimeData;
}

public class ManiShardInstance : ItemInstance
{
    public float? TimerStartedAt;    // null when not armed
    public bool IsArmed => TimerStartedAt.HasValue;
}
```

---

## 7. Procedural Level Assembly

Same structure as before, with **Mani vein anchors** added to room metadata and **faction-aware encounter director**.

### 7.1 Room Prefab Contract

```csharp
public class RoomMetadata : MonoBehaviour
{
    public string RoomId;
    public string SubTheme;                 // "MiningTunnel" / "DrillRoom" / "OutdoorCamp" / "ManiVeinChamber"
    public RoomSize Size;
    public RoomRole Role;                   // Combat / Loot / Corridor / Boss / Evac / Hub
    public Vector2Int FootprintCells;       // for assembler tile-fit (continuous combat ≠ no cell grid for assembly)
    public List<DoorSocket> DoorSockets;
    public List<CoverAnchor> CoverAnchors;
    public List<SpawnAnchor> SpawnAnchors;
    public List<TraversalPoint> Traversals;
    public List<HeightMarker> HeightLayers;
    public List<ManiVeinAnchor> ManiVeins;       // NEW
    public List<HazardTrigger> HazardTriggers;         // NEW — e.g., cave-in zones
    public Bounds RoomBounds;
}

[Serializable]
public class CoverAnchor
{
    public Vector3 LocalPosition;
    public CoverType Type;           // Full / Half
    public Vector3 FacingDirection;  // NEW — direction the cover protects against
    public float FacingConeDegrees;  // NEW — cone half-angle; shots outside cone = flanked
    public float ProtectionRadius;   // how close a combatant must be to claim this cover
}

[Serializable] public class SpawnAnchor      { public Vector3 LocalPosition; public SpawnRole Role; public string Tag; }
[Serializable] public class TraversalPoint   { public Vector3 EntryLocal; public Vector3 ExitLocal; public TraversalKind Kind; }
[Serializable] public class ManiVeinAnchor{ public Vector3 LocalPosition; public int ShardYield; public CoverType ProvidedCover; }
[Serializable] public class HazardTrigger    { public Vector3 LocalPosition; public HazardKind Kind; public float TriggerChance; }
```

Editor validator (`Gameplay.Editor`) refuses to mark a prefab "ready" if metadata is missing.

### 7.2 Assembler, Encounter Director, Loot Director

```csharp
public class LevelAssembler : ILevelAssembler
{
    public AssembledLevel Assemble(MapDef seed, int randomSeed) { ... }
}

public class EncounterDirector : IEncounterDirector
{
    readonly IFactionRegistry _factions;

    public void PopulateEnemies(AssembledLevel level, int difficulty, int rngSeed)
    {
        foreach (var room in level.CombatRooms)
        {
            var faction = _factions.Get("Grinders");
            int enemyBudget = DifficultyCurve.Enemies(difficulty, room.Size);
            int coverClusters = ClusterCoverAnchors(room.Metadata.CoverAnchors);
            int sightlines    = room.CoverGraph.PrimarySightlines.Count;
            // Place Grinder pack across cover clusters; reserve 1 sniper slot per sightline;
            // chance to add a Chief or Shaman at higher difficulty.
            ...
        }
    }
}

public class LootDirector : ILootDirector
{
    public void PopulateLoot(AssembledLevel level, int difficulty, int rngSeed)
    {
        // Standard loot table draws onto SpawnAnchor.Role = Loot
        // Mani shards spawn at ManiVeinAnchors based on map's ManiVeinDensityCurve
        ...
    }
}
```

### 7.3 Runtime Navmesh

`NavMeshSurface.BuildNavMeshAsync()` during `RaidLoadingState`. Combat AI and player-controlled real-time movement both use this navmesh.

---

## 8. Inventory System (Pattern: Composite + Model/View)

Unchanged from prior spec. Pure C# model, no MonoBehaviour.

```csharp
public class GridContainer { ... }
public class Inventory     { ... }
```

Mani shards take a dedicated slot type — they can't stack with normal items and can be auto-detected for grenade priming.

---

## 9. Stats & Damage Pipeline (Pattern: Strategy + Chain of Responsibility)

Damage pipeline = ordered chain. Adding new effects = drop in a step.

```
RawDamage
  → CoverStep              (apply Full/Half/None multiplier; query CoverAnchor.FacingCone for flank)
  → HeightStep             (apply +10% / ignore-half-cover)
  → PenetrationStep        (vs. armor class)
  → BodyPartStep           (head/torso/limb multiplier)
  → ManiProcStep        (NEW — if attack triggers Mani effect, queue for resolution)
  → CritStep               (flank / perfect-parry / status-combo crits)
  → ParryStep              (read queued reactive — 0 / 0.5 / 1.0 multiplier)
  → StatusApplicationStep  (roll status proc, queue StatusEffect)
  → Final
```

`CoverStep` evaluates `CoverAnchor.FacingCone` to determine flank state — this is how flanking is preserved in continuous space.

---

## 10. Exploration Mode

Unchanged from prior spec. PlayerController + goal-oriented AI brain. AI publishes `EncounterTriggered` when alerted with LOS.

---

## 11. Combat Mode — Continuous Tactical Space (Sparks of Hope × XCOM × E33)

### 11.1 TacticalSpace (replaces TacticalGrid — continuous, NOT a grid)

```csharp
public class TacticalSpace
{
    public Bounds EncounterBounds;
    public NavMeshSurface NavMesh;
    public List<CoverAnchor> WorldCoverAnchors;     // baked from rooms in scope
    public List<TraversalPoint> WorldTraversals;
    public List<ManiVeinAnchor> WorldManiVeins;

    public bool CanReach(Vector3 from, Vector3 to, float budget);
    public Vector3[] FindPath(Vector3 from, Vector3 to, float maxLength);
    public CoverAnchor ClaimedCoverFor(Combatant actor);
    public LineOfSight CheckLOS(Vector3 from, Vector3 to);
}
```

**Continuous coords.** No tile snap. NavMesh handles pathing. Cover is anchor-based (with facing cones — see 11.2).

### 11.2 CoverSystem (XCOM crispness rescued)

```csharp
public class CoverSystem
{
    public CoverState Evaluate(Combatant attacker, Combatant defender, TacticalSpace space)
    {
        var anchor = space.ClaimedCoverFor(defender);
        if (anchor == null) return CoverState.None;

        Vector3 toAttacker = (attacker.Position - defender.Position).normalized;
        float angle = Vector3.Angle(anchor.FacingDirection, toAttacker);

        bool insideCone = angle <= anchor.FacingConeDegrees;
        bool heightAdv  = attacker.HeightLayer > defender.HeightLayer;

        if (!insideCone) return CoverState.Flanked;             // shot from outside cone → flank
        if (heightAdv && anchor.Type == CoverType.Half) return CoverState.None;
        return anchor.Type == CoverType.Full ? CoverState.Full : CoverState.Half;
    }
}
```

Damage pipeline's `CoverStep` queries this. **Flanking is preserved without a grid** because cover anchors carry their own facing.

### 11.3 Combat Controller

```csharp
public class CombatController : MonoBehaviour
{
    Queue<Combatant> _turnOrder;
    TacticalSpace _space;
    public Combatant Current { get; private set; }
    public CombatPhase Phase { get; private set; }

    public void Begin(EncounterData data)
    {
        _space = TacticalSpace.BuildFromAssembledRooms(data.Rooms);
        _turnOrder = BuildInitiative(data.Combatants);
        NextTurn();
    }
}
```

### 11.4 Combatant

```csharp
public abstract class Combatant
{
    public StatBlock Stats;
    public int ActionsRemaining;
    public bool MovementLocked;             // true after weapon fire (commit-to-shot rule)
    public Vector3 Position;                // CONTINUOUS, not grid
    public int HeightLayer;
    public List<AbilityDef> KnownAbilities;
    public Dictionary<string, int> AbilityCooldowns;
    public List<StatusEffectInstance> ActiveStatuses;
    public IntentDeclaration NextIntent;
    public ReactiveQueue Reactive;
    public ManiShardInstance HeldShard;  // non-null when shard is live in hand
}

public class PlayerCombatant : Combatant { public Inventory Loadout; public float UltimateGauge; }
public class EnemyCombatant  : Combatant { public EnemyDef Def; public AIGoalProfile Goals; public FactionDef Faction; }
```

### 11.5 Actions (Pattern: Command)

```csharp
public interface ICombatAction
{
    int ActionCost { get; }
    bool CanExecute(CombatContext ctx);
    IEnumerator Execute(CombatContext ctx);
}

public class ShootAction      : ICombatAction { ... }   // sets MovementLocked = true on resolve
public class AbilityAction    : ICombatAction { ... }
public class OverwatchAction  : ICombatAction { ... }   // ends turn with held trigger
public class UseItemAction    : ICombatAction { ... }
public class ReloadAction     : ICombatAction { ... }
public class FleeAction       : ICombatAction { ... }
public class ParryAction      : ICombatAction { ... }   // queues reactive, free
public class DodgeAction      : ICombatAction { ... }   // queues reactive, free, stamina cost
public class ThrowManiGrenadeAction : ICombatAction { ... } // see Section 12
public class DetonateInHandAction      : ICombatAction { ... }
```

Movement is **not** an `ICombatAction` — it's a real-time controller (see 11.6).

### 11.6 MovementController — real-time stick input during your turn (Sparks of Hope)

```csharp
public class MovementController : MonoBehaviour
{
    PlayerCombatant _actor;
    float _budgetRemaining;
    int _dashCharges;

    public void BeginTurn(PlayerCombatant actor)
    {
        _actor = actor;
        _budgetRemaining = actor.Stats.MoveRange.Value;
        _dashCharges = actor.Stats.DashCount.Value;
        ShowMovementDome(actor.Position, _budgetRemaining);
    }

    void Update()
    {
        if (_actor.MovementLocked) { HideDome(); return; }
        var stickInput = InputManager.MoveAxis;
        var newPos = NavMeshMove(_actor.Position, stickInput, Time.deltaTime * MoveSpeed);
        float consumed = Vector3.Distance(_actor.Position, newPos);
        if (_budgetRemaining - consumed >= 0f)
        {
            _actor.Position = newPos;
            _budgetRemaining -= consumed;
            CheckDashThrough(newPos);
            CheckTraversalPickup(newPos);
        }
    }
}
```

`Team Jump` for the main game layers onto this controller — detects allied combatants on stick-direction → offers launchpad → free reposition without consuming budget. **Additive, not a rewrite.**

### 11.7 Intent System (Pattern: Strategy on Enemy AI + Faction Overlay)

```csharp
public class EnemyIntentPicker
{
    public IntentDeclaration Pick(EnemyCombatant enemy, CombatContext ctx)
    {
        var goalWeights = enemy.Goals.Evaluate(ctx);
        var factionOverlay = enemy.Faction?.AIOverlay;
        if (factionOverlay != null) factionOverlay.Apply(goalWeights, ctx);
        // pack-tribe overlay biases toward Regroup / CoordinatedFlank when allies nearby
        return BuildIntent(enemy, goalWeights, ctx);
    }
}

public readonly struct IntentDeclaration
{
    public readonly AbilityDef Action;
    public readonly Combatant Target;
    public readonly Vector3? TargetPosition;
    public readonly int PredictedDamage;
    public readonly IntentIcon Icon;
}
```

### 11.8 Reactive Layer (Parry / Dodge — Expedition 33)

Unchanged from prior spec. Parry input handler grades timing → modifies damage in the `ParryStep`.

### 11.9 StatusEffectSystem (Sparks of Hope deck)

```csharp
public class StatusEffectSystem
{
    public void Apply(Combatant target, StatusEffectDef def, int duration);
    public void TickTurnEnd(Combatant actor);
    public bool BlocksAction(Combatant actor);
    public bool BlocksMovement(Combatant actor);
}
```

Combos: `StatusApplied(Burn)` on a target already affected by `Push` → publishes `StatusComboTriggered` for bonus damage.

### 11.10 OverwatchSystem (XCOM, adapted for continuous LOS)

```csharp
public class OverwatchSystem
{
    readonly List<OverwatchSlot> _active = new();

    public void Hold(Combatant actor);

    // Called each FixedUpdate while an enemy is moving in real-time
    public void TickEnemyMovement(Combatant moving)
    {
        foreach (var slot in _active)
        {
            if (slot.Actor.IsDead) continue;
            if (CheckContinuousLOS(slot.Actor, moving))
            {
                FireOverwatch(slot, moving);
                _active.Remove(slot);
                break;
            }
        }
    }
}
```

Continuous LOS check is the price we pay for continuous space. Raycast per tick during enemy movement; trivial at prototype scale.

### 11.11 Turn State Machine

```
TurnStart → ApplyTurnStartStatuses → (Enemy? DeclareIntent : EnableMovementController) →
PlayerCommitsAction → ResolveAction (MovementLocked = true if Shoot) → ApplyEffects →
CheckEnd → NextTurn
```

---

## 12. Mani System (Gameplay.Mani)

The signature mechanic. Lore-justified, mechanically central.

### 12.1 ManiSystem

```csharp
public interface IManiSystem : IService
{
    void ArmShard(ManiShardInstance shard, Combatant holder);
    void ThrowShard(ManiShardInstance shard, Vector3 target);
    void DetonateInHand(ManiShardInstance shard);
    void Tick(float dt);    // counts down armed timers, auto-detonates at 0
    ManiEffectDef RollEffect(ManiShardKind kind);
}

public class ManiSystem : IManiSystem
{
    readonly ManiEffectTable _table;
    readonly List<LiveShard> _live = new();

    public ManiEffectDef RollEffect(ManiShardKind kind)
    {
        if (kind == ManiShardKind.Refined) /* return chosen — main game only */ ;
        // Raw: roll based on table weights (Common/Uncommon/Rare)
        var tier = SampleTier(_table.TierWeightCurve);
        return SampleFromTier(tier);
    }

    public void Tick(float dt)
    {
        foreach (var live in _live)
        {
            live.Remaining -= dt;
            if (live.Remaining <= 0f) DetonateInHand(live.Shard);  // auto-detonate self-damage
        }
    }
}
```

### 12.2 Detonation Flow

```csharp
public class ManiDetonationResolver
{
    public void Detonate(Vector3 position, ManiEffectDef effect)
    {
        SpawnVfx(effect.VfxPrefab, position);
        var affected = OverlapSphere(position, effect.Radius);
        foreach (var target in affected)
        {
            if (effect.Status != null)
                Services.Get<StatusEffectSystem>().Apply(target, effect.Status, effect.Status.DurationTurns);
            foreach (var custom in effect.CustomEffects)
                custom.Apply(target);
        }
        Services.Get<IEventBus>().Publish(new ManiGrenadeDetonated(position, effect));
    }
}
```

### 12.3 Mani Veins (Destructible cover, lore tie)

```csharp
public class ManiVein : MonoBehaviour
{
    public ManiVeinAnchor Anchor;
    public CoverType ProvidedCover;
    public int Hp;

    public void OnDamaged(DamageInfo dmg)
    {
        Hp -= dmg.Amount;
        if (Hp <= 0) Shatter();
    }

    void Shatter()
    {
        // Drop shards as loot
        for (int i = 0; i < Anchor.ShardYield; i++) DropShard(transform.position);
        // Trigger radial random Mani effect (risk/reward)
        var rolled = Services.Get<IManiSystem>().RollEffect(ManiShardKind.Raw);
        new ManiDetonationResolver().Detonate(transform.position, rolled);
        Services.Get<IEventBus>().Publish(new ManiVeinBroken(transform.position, rolled));
    }
}
```

### 12.4 Mani-ignorant factions (Grinders self-harm)

`FactionDef.IsManiIgnorant = true` for Grinders. When a Grinder throws raw Mani, the AI uses `RollEffect` exactly like the player — same random table, same self-harm risk. **The mechanic emerges from data, not special-case code.**

---

## 13. Faction System (Gameplay.Faction)

### 13.1 FactionRegistry & FactionDef

```csharp
public interface IFactionRegistry : IService
{
    FactionDef Get(string factionId);
    int GetReputation(string factionId);
    void AdjustReputation(string factionId, int delta);
}
```

### 13.2 AI Overlay (pack-tribe behavior)

```csharp
public class AIOverlayProfile : ScriptableObject
{
    public float RegroupBias;
    public float CoordinatedFlankBias;
    public float NeverAlonePenalty;     // negative weight on actions that isolate
    public float CalloutFrequency;

    public void Apply(GoalWeights weights, CombatContext ctx)
    {
        // bias goal weights based on ally proximity, alert state, etc.
    }
}
```

Grinders' overlay heavily biases `Regroup` + `CoordinatedFlank` when allies are within X meters; `NeverAlonePenalty` discourages an isolated Grinder from charging alone.

### 13.3 Reputation

`FactionReputationChanged` events fire on kill / spare / quest / trade. Vendor inventory + pricing read reputation. Skill tree branches can require minimum reputation.

---

## 14. Mode Transition (Explore ↔ Combat)

```csharp
class ExploreState : IGameState
{
    public void Enter() =>
        Services.Get<IEventBus>().Subscribe<EncounterTriggered>(OnEncounter);

    void OnEncounter(EncounterTriggered e) =>
        Services.Get<IGameStateMachine>().ChangeTo<CombatState>();
}
```

`CombatState.Enter`:
1. Freezes enemies (per-system pause flag). Player stays controllable.
2. Builds `TacticalSpace` from rooms in scope (current + adjacent within ~15m).
3. **No position snap** — combatants keep continuous coords.
4. Cinemachine blends camera (~0.8s).
5. Combat UI canvas fades in (movement dome appears on player's first turn).
6. `CombatController.Begin(encounterData)`.

On `CombatEnded`, publishes result, returns to `ExploreState`. Mani grenades live across the transition — if you've armed a shard and combat ends, the timer keeps ticking in real time. Pure tension.

---

## 15. Base Layer

| Feature        | Controller (MB)    | Service (C#)       | Data SO            |
|---|---|---|---|
| Stash          | `StashScreen`      | `StashService`     | — (player save)    |
| Vendor (Rogue Grinder) | `VendorScreen` | `VendorService` | `VendorDef` (references `FactionDef`) |
| Crafting       | `CraftScreen`      | `CraftService`     | `RecipeDef`        |
| Loadout        | `LoadoutScreen`    | uses Inventory     | —                  |
| Knowledge Log  | `LogScreen`        | `KnowledgeService` | `LoreEntryDef`     |

Vendor pricing reads `IFactionRegistry.GetReputation("RogueGrinder")` (or Grinders broadly).

---

## 16. Save System (Pattern: Memento + Registry)

```csharp
public interface ISaveable
{
    string SaveKey { get; }
    JObject CaptureState();
    void RestoreState(JObject state);
}
```

All items reference defs by stable string id. Save files survive asset reorganization. Persisted: character, stash, vendor rep, **faction reputations**, skill tree, **knowledge log**, settings.

No mid-raid save. Saves at: raid start, raid end, base transactions.

---

## 17. UI Architecture (Pattern: MVP)

Combat UI is the heaviest screen — event-driven throughout:
- `TurnStarted` → updates initiative bar, shows/hides movement dome
- `IntentDeclared` → spawns intent icon above enemy
- `StatusApplied` → adds status badge
- `ParryResolved` → triggers parry feedback flash
- `ManiGrenadeArmed` → shows live timer with ticking SFX
- `ManiGrenadeDetonated` → spawns floating effect-name + VFX
- `ManiVeinBroken` → screen shake + radial warning indicator

---

## 18. Project Folder Layout

```
Assets/
  _Project/
    Art/        Audio/        Prefabs/        Scenes/
    Rooms/                       ← room prefab library by sub-theme
      MiningTunnel/  DrillRoom/  OutdoorCamp/  ManiVeinChamber/
  Code/
    Core/                        (asmdef: Game.Core)
    Data/                        (asmdef: Game.Data)
    Gameplay.Shared/
    Gameplay.Explore/
    Gameplay.Combat/
    Gameplay.Procgen/
    Gameplay.Mani/            ← NEW
    Gameplay.Faction/            ← NEW
    Gameplay.Base/
    UI/
    Editor/
  Data/                          (ScriptableObject assets)
    Items/  Weapons/  Ammo/  Armor/  Abilities/  StatusEffects/
    Enemies/  Recipes/  LootTables/  Maps/  AssemblerRules/
    ManiEffects/                 ← NEW
    Factions/                    ← NEW (Grinders / RivalScavengers / RogueGrinder)
  Settings/                      (URP, Input Actions, Addressables config)
```

---

## 19. Patterns Used — Quick Reference

| Pattern           | Where                                                          |
|---|---|
| Service Locator   | `Services` static + `GameBootstrapper`                         |
| State             | `GameStateMachine`, Player FSM, AI FSM, Turn FSM               |
| Pub/Sub Event Bus | Cross-system events (immutable structs)                        |
| Strategy          | Damage steps, intent picker, encounter director heuristics, **AI overlay (faction)** |
| Command           | Combat actions (`ICombatAction`)                               |
| Composite         | Grid containers within containers                              |
| Flyweight         | `ItemDef` / `ManiEffectDef` / `FactionDef` shared vs runtime instances |
| Chain of Resp.    | Damage pipeline (`IDamageStep` chain)                          |
| Builder           | `LevelAssembler` stitches rooms                                |
| Memento           | Save/load via `ISaveable.CaptureState`                         |
| MVP               | UI presenter/view/model split                                  |
| Decorator         | AI Overlay applied on top of base goal profile                 |
| Object Pool       | Bullets/VFX/floating damage numbers, status badge UI           |

---

## 20. Build Order (Suggested Sprints)

> **REVISED 2026-06-02** to the locked combat model. **#1 priority is the dual-camera reactive-loop gray-box** — if that single loop feels good, the combat identity is validated.

1. **GRAY-BOX THE DUAL-CAMERA REACTIVE LOOP FIRST** — one authored tile arena, 1 simple enemy, side-based phases, plan in tilted-3/4 → enemy attack → OTS reaction-cam → parry/dodge → snap back. The make-or-break.
2. **Combat core** — `TileGrid`, facing/flank arcs, AP economy (parry-fed, no carryover), splittable movement, flat deterministic damage pipeline (Front/Side/Rear→crit, no armor), intent telegraphs.
3. **Reactive layer** — Dodge + Parry (Block/Perfect), counter + flat AP, Perfect = crit + drops raw Mani, ranged deflect.
4. **Bootstrap + Data layer + Inventory grid + Stash + Loadout.** The loot/item bones.
5. **Exploration + player controller + world loot + stealth AI + Encounter-Arena transition** (explore → authored arena → return).
6. **Mani System / economy** — raw Mani loot, **refining minigame**, refined Mani, raw-Mani launcher (Effect Table), **research minigame**, vein/big-ore. **The signature loop.**
7. **Status effect deck (prototype core)** — Burn/Freeze/Stagger/Push + combo-crit (Freeze→Shatter).
8. **Per-biome enemy roster** (designed from gray-box results) + pack-tribe AI + hazard-push.
9. **Authored arena pool** (Lithic Mow themed) — expand the set; boss bespoke staging.
10. **Rogue Grinder vendor + crafting + faction reputation + Knowledge Log.**
11. **AI polish, raid timer, evac, audio pass, tutorial raid.**
12. **V1 features from [[09 Complete Feature List]] in priority order.**
*(Cut from the old plan: procgen MVP, cover system, overwatch, ultimate gauge, live-timer grenade.)*

---

## 21. Open Questions to Pin Down Before Sprint 1

*(Most of the original questions are now RESOLVED or MOOT after the combat redesign — see [[06 Element in Looter Shooter|06]]):*
- ~~Camera angle~~ → **RESOLVED:** one unified **perspective** rig; **tactical = OTS reaction-cam angle + 15–25°** (tactical ~45–55°, OTS ~25–35°). Not top-down, not orthographic.
- ~~Movement dome visualization~~ → **MOOT** (grid, no dome).
- ~~Mani grenade timer length~~ → **MOOT** (held-timer grenade retired; raw Mani is fired AoE via the launcher).
- **Real-time → combat transition style:** the **Encounter-Arena** transition (explore → load a self-contained arena → return on win). Blend style still TBD in build.
- ~~Encounter scope visualization~~ → **MOOT** (combat is a separate arena, not in-place).
- **Still open (build-time):** the dual-camera blend feel (the #1 gray-box), exact AP/range/cost numbers, the two Mani minigames' designs.

Resolve the build-time items during the gray-box, not before.

---

## 22. Carry-Forward Checklist (Prototype → Main Game)

Every system listed transfers to the dream game with **additive changes only, no rewrites**:

| System                          | Prototype | Main Game additions                          |
|---|---|---|
| Combat controller / turn FSM (side-based) | Solo | Party-of-3 turn slots                  |
| TileGrid + facing/flank         | Same     | Larger arenas, more enemies                   |
| Movement (grid budget, splittable) | Same  | + party positioning                          |
| Dual-camera rig (tactical ↔ OTS) | Same    | + party-target framing                       |
| AP / parry economy              | Same     | + class resources                            |
| Status effect deck              | Same     | + class-specific + compound-spell effects     |
| Intent telegraphs               | Same     | + party-target prediction UI                  |
| **Mani System / economy**       | Raw loot → refine minigame → refined fuel; research minigame; Bhu only | + Jal/Vayu/Agni + 2-element compounds + purification anchor |
| **Faction System**              | Grinders + Rival Scavengers + RogueGrinder | + Amphibian Husks + Reptile Husks + purification system |
| Authored arena pool             | Lithic Mow themed | + Vats/Forge themed arenas (procgen = dream-game dungeons, NOT carried from here) |
| Inventory grid                  | Same     | + per-character + shared stash split          |
| Damage pipeline (flat, flank/crit, no armor) | Same | + party-buff steps                    |
| Save system                     | Same     | + city/automation state registered            |

If a sprint produces a system that *can't* go on this list, stop and rethink it.
