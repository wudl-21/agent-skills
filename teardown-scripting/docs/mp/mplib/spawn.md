# Module: spawn

Server-side player spawning and respawning system.

- Supports timed auto-respawns, forced respawns, team-based spawn groups, and configurable loadouts
- State synced via `shared.respawnTimeLeft`
- **Server-only** (all functions)

**Navigation:** [_INDEX](_INDEX.md) | [countdown](countdown.md) | [eventlog](eventlog.md) | [hud](hud.md) | [spectate](spectate.md) | [stats](stats.md) | [teams](teams.md) | [tools](tools.md) | [ui](ui.md) | [util](util.md)

---

## Functions

### [API] spawnInit()
*(server)* Reset all internal spawn state. Call once during game mode setup.

---

### [API] spawnSetSpawnTransforms(spawnTransforms[, groupIndex])
*(server)* Define spawn points. Transforms are `{pos, rot}` tables (standard API format).  
If `groupIndex` is omitted, all points go to group 1 and previous groups are cleared.

| Param | Type | Description |
|---|---|---|
| `spawnTransforms` | table | List of spawn transforms |
| `groupIndex` | number | *(optional)* Group ID (default: 1) |

```lua
-- 2-team example
spawnSetSpawnTransforms(spawnTransformTeam1, 1)
spawnSetSpawnTransforms(spawnTransformTeam2, 2)
```

---

### [API] spawnSetRespawnTime(respawnTime)
*(server)* Set auto-respawn delay. Controls how long a dead player waits before `spawnTick` respawns them.

| Param | Type | Description |
|---|---|---|
| `respawnTime` | number | Seconds before auto-respawn |

---

### [API] spawnSetDefaultLoadout(loadout)
*(server)* Set default loadout applied at respawn. All tools cleared first, then loadout applied in order. First entry becomes active tool.

| Param | Type | Description |
|---|---|---|
| `loadout` | table | List of `{toolName, ammoCount}` pairs |

```lua
function server.init()
    spawnSetDefaultLoadout({
        {"gun",         7},
        {"sledge",      0},
        {"spraycan",    0},
        {"extinguisher",0},
    })
end
```

---

### [API] spawnSetRespawnAtCurrentLocation(active)
*(server)* When enabled, players respawn at their death position instead of spawn points.

| Param | Type | Description |
|---|---|---|
| `active` | bool | `true` to enable |

---

### [API] spawnGetPlayerRespawnTimeLeft(player)
Get respawn time remaining. Returns 0 when player is alive.

| Param | Type | Description |
|---|---|---|
| `player` | number | Player ID |

**Returns:** `number` — Seconds until respawn, or 0 if alive.

---

### [API] spawnSpawnPlayer(transform, loadout, player)
*(server)* Immediately respawn a player at a given transform with a given loadout.  
Pass `nil` for transform to use engine default. Pass `nil` for loadout to skip tool changes.

| Param | Type | Description |
|---|---|---|
| `transform` | table\|nil | Spawn transform |
| `loadout` | table\|nil | `{{toolName, ammoCount}, ...}` |
| `player` | number | Player ID |

---

### [API] spawnRespawnPlayer(playerId)
*(server)* Flag a player for forced respawn on the next `spawnTick`. Ignores death timer.

| Param | Type | Description |
|---|---|---|
| `playerId` | number | Player ID |

---

### [API] spawnRespawnAllPlayers()
*(server)* Flag all players for forced respawn on the next `spawnTick`. Useful at round start.

---

### [API] spawnTick(dt[, playerGroupList])
*(server)* Main update loop. Call every tick. Handles auto-respawn timing, forced respawns, and group-based spawn point selection.

| Param | Type | Description |
|---|---|---|
| `dt` | number | Time step in seconds |
| `playerGroupList` | table | *(optional)* `{[playerId] = groupIndex, ...}` |

```lua
-- Team-based example
local playerGroupList = {
    [1]=1, [2]=1, [3]=1,  -- team 1
    [4]=2, [5]=2, [6]=2,  -- team 2
}
spawnTick(dt, playerGroupList)
```

---

### [API] spawnGetSpawnTransformGroups()
*(server)* Get all registered spawn transform groups.

**Returns:** `table` — `{groupIndex → {transforms}}` mapping.
