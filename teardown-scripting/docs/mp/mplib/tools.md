# Module: tools

Server-side tool, loot crate, and dropped-weapon management for multiplayer.

**Responsibilities:**
- Define and manage respawnable loot tiers at spawn points
- Drop tools with remaining ammo when players die
- Spawn/manage physical tool bundle entities (crates + loose drops)
- Handle player interaction with pickup bodies

**All functions are server-side unless noted.**

**Navigation:** [_INDEX](_INDEX.md) | [countdown](countdown.md) | [eventlog](eventlog.md) | [hud](hud.md) | [spawn](spawn.md) | [spectate](spectate.md) | [stats](stats.md) | [teams](teams.md) | [ui](ui.md) | [util](util.md)

---

## Functions

### [API] toolsInit()
*(server)* Reset all loot tiers and tool bundles to defaults. Precomputes tool pickup data.

---

### [API] toolsSetRespawnTime(respawnTime)
*(server)* Set loot respawn time for all tiers (how long after pickup/despawn before a new tool spawns).

| Param | Type | Description |
|---|---|---|
| `respawnTime` | number | Seconds before loot respawns |

---

### [API] toolsSetDropToolsOnDeath(dropTools)
*(server)* Toggle tool drops on player death.

| Param | Type | Description |
|---|---|---|
| `dropTools` | boolean | `true` to enable world drops on death |

---

### [API] toolsPreventToolDrop(toolId)
*(server)* Mark a specific tool as non-droppable even when `toolsSetDropToolsOnDeath(true)`.

| Param | Type | Description |
|---|---|---|
| `toolId` | string | Tool identifier |

---

### [API] toolsAddModToolsToLootTable(lootTable[, weight])
Add all mod-defined tools (`game.tool[*].custom = true`) to a loot table if not already present. Can be called server or client; typically used server-side.

| Param | Type | Description |
|---|---|---|
| `lootTable` | table | Loot entry list to extend |
| `weight` | number | *(optional)* Spawn weight per tool (default: 3) |

---

### [API] toolsAddLootTier(transforms, lootTable)
*(server)* Register a loot tier: a set of spawn locations with an associated weighted loot table.

| Param | Type | Description |
|---|---|---|
| `transforms` | table | List of `TTransform` spawn locations |
| `lootTable` | table | List of `{name, weight[, amount]}` loot entries |

```lua
-- Loot table entry fields:
--   name   (string) — tool ID
--   weight (number) — relative spawn probability
--   amount (number) — ammo/pickup count (optional)

lootTables = {}
lootTables[1] = {
    {name="steroid", weight=10, amount=4},
    {name="plank",   weight=2,  amount=5},
}
lootTables[2] = {
    {name="shotgun", weight=7},
    {name="gun",     weight=7},
    {name="bomb",    weight=5},
}
lootTables[3] = {
    {name="rifle",     weight=9},
    {name="pipebomb",  weight=5},
    {name="rocket",    weight=10},
    {name="explosive", weight=5},
}

toolsAddLootTier(toolSpawns[1], lootTables[1])
toolsAddLootTier(toolSpawns[2], lootTables[2])
toolsAddLootTier(toolSpawns[3], lootTables[3])
```

---

### [API] toolsCleanup()
*(server)* Delete all spawned tool entities (`_cleanUpToolBundle`) and clear loot tier definitions. Call before starting a new match or re-init.

---

### [API] toolsTick(dt)
*(server)* Main update loop. Call once per frame.

- Despawns tool bundles when all players are out of range
- Ticks loot tiers, spawns new crates when timers expire
- Drops tools on player death (if enabled)
- Handles player interaction with ammo/tool pickup bodies

| Param | Type | Description |
|---|---|---|
| `dt` | number | Delta time in seconds |
