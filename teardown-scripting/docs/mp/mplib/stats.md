# Module: stats

Tracks per-player kill and death counts in a multiplayer session.

- `statsTick` must run **server-side**
- `statsGetKills` / `statsGetDeaths` usable on both server and client

**Navigation:** [_INDEX](_INDEX.md) | [countdown](countdown.md) | [eventlog](eventlog.md) | [hud](hud.md) | [spawn](spawn.md) | [spectate](spectate.md) | [teams](teams.md) | [tools](tools.md) | [ui](ui.md) | [util](util.md)

---

## Functions

### [API] statsInit()
*(server)* Clear all recorded stats. Call at match/round start.

---

### [API] statsTick(playerTeamList)
*(server)* Process `playerdied` events and update kill/death counts. Call once per tick during active match.

| Param | Type | Description |
|---|---|---|
| `playerTeamList` | table\|nil | `{[playerId] = teamId, ...}` — nil for free-for-all |

```lua
-- Team mode
local playerGroupList = {[1]=1,[2]=1,[3]=1,[4]=2,[5]=2,[6]=2}
statsTick(playerGroupList)

-- Free-for-all
statsTick(nil)
```

> **[CONSTRAINTS]**
> - Kills from teammates are NOT counted (when `playerTeamList` is provided and attacker/victim share same team)

---

### [API] statsGetKills(playerId)
Get kill count for a player. Returns 0 if no stats recorded.

| Param | Type | Description |
|---|---|---|
| `playerId` | number | Player ID |

**Returns:** `number`

---

### [API] statsGetDeaths(playerId)
Get death count for a player. Returns 0 if no stats recorded.

| Param | Type | Description |
|---|---|---|
| `playerId` | number | Player ID |

**Returns:** `number`
