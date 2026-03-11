# Module: teams

Dynamic team assignment, player coloring, and team selection UI for multiplayer matches.

- **Server-side:** team state, assignment, tick logic
- **Client-side:** team selection UI draw

**Navigation:** [_INDEX](_INDEX.md) | [countdown](countdown.md) | [eventlog](eventlog.md) | [hud](hud.md) | [spawn](spawn.md) | [spectate](spectate.md) | [stats](stats.md) | [tools](tools.md) | [ui](ui.md) | [util](util.md)

---

## Functions

### [API] teamsInit(teamCount)
*(server)* Initialize team system. Sets default names/colors, clears previous state.

| Param | Type | Description |
|---|---|---|
| `teamCount` | number | Number of teams to create |

---

### [API] teamsGetColor(teamId)
Get color for a team. Falls back to default color if team not configured.

| Param | Type | Description |
|---|---|---|
| `teamId` | number | Team ID (1-based) |

**Returns:** `{r,g,b}` — components in [0,1].

---

### [API] teamsSetColors(colors)
*(server)* Set custom colors for all teams.

| Param | Type | Description |
|---|---|---|
| `colors` | table | List of `{r,g,b}` per team, e.g. `{{1,0,0},{0,1,0}}` |

---

### [API] teamsSetNames(names)
*(server)* Set custom names for all teams.

| Param | Type | Description |
|---|---|---|
| `names` | table | List of strings, e.g. `{"Red","Blue","Green"}` |

---

### [API] teamsGetName(teamId)
Get team name. Falls back to default if not configured.

| Param | Type | Description |
|---|---|---|
| `teamId` | number | Team ID (1-based) |

**Returns:** `string`

---

### [API] teamsSetTeams(teams)
*(server)* Directly assign players to teams. Replaces current assignments.

| Param | Type | Description |
|---|---|---|
| `teams` | table | `{{playerId,...}, {playerId,...}}` — one list per team |

```lua
-- e.g. players 1,2 on team1; players 3,4 on team2
teamsSetTeams({ {1,2}, {3,4} })
```

---

### [API] teamsGetPlayerTeamsList()
Build lookup of `{[playerId] = teamId, ...}`. Useful for broadcasting team membership.

**Returns:** `table` — e.g. `{[1]=2, [2]=1, ...}`

---

### [API] teamsGetTeamPlayers(teamId)
Get players in a specific team.

| Param | Type | Description |
|---|---|---|
| `teamId` | number | Team ID (1-based) |

**Returns:** `table` — List of player IDs (empty if team doesn't exist).

---

### [API] teamsGetPlayerColorsList()
Build lookup of `{[playerId] = {r,g,b}, ...}` from current team assignments.

**Returns:** `table`

```lua
-- Format:
-- { [1]={0.2,0.55,0.8}, [2]={0.8,0.25,0.2}, ... }
```

---

### [API] teamsStart(skipCountdown)
*(server)* Start match or begin team selection countdown.

- `skipCountdown = true` → auto-assign teams immediately; setup complete
- `skipCountdown = false` → render `teamsDraw` to allow player team selection

| Param | Type | Description |
|---|---|---|
| `skipCountdown` | bool | Skip team selection phase |

---

### [API] teamsIsSetup()
Check if team setup is complete.

**Returns:** `bool` — `true` if setup done.

---

### [API] teamsGetTeamId(playerId)
Get the team ID for a player.

| Param | Type | Description |
|---|---|---|
| `playerId` | number | Player ID |

**Returns:** `number` — Team ID (1-based), or `0` if unassigned.

---

### [API] teamsTick(dt)
*(server)* Main team logic update. Call once per tick. Handles connect/disconnect, assigns teams when setup completes, posts `teamsupdated` event.

| Param | Type | Description |
|---|---|---|
| `dt` | number | Time step in seconds |

**Returns:** `bool` — `true` if pending game start triggered this tick.

---

### [API] teamsGetLocalTeam()
*(client)* Get players on the same team as the local player.

**Returns:** `table` — List of player IDs.

---

### [API] teamsDraw(dt)
*(client)* Render team selection screen UI. Players pick a team to join. Early-outs once setup is complete.

| Param | Type | Description |
|---|---|---|
| `dt` | number | Time step in seconds |
