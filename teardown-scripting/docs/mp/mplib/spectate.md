# Module: spectate

Client-side spectator system for multiplayer. Activates automatically when the local player dies.

**Features:**
- Third-person camera with smooth transition into spectate mode
- Mouse orbit + zoom
- Left mouse = next player, Right mouse = previous player
- Optional map handling, outlines for spectated player/vehicle
- Tracks attacker for brief outline highlight

All functions are **client-side only**.

**Navigation:** [_INDEX](_INDEX.md) | [countdown](countdown.md) | [eventlog](eventlog.md) | [hud](hud.md) | [spawn](spawn.md) | [stats](stats.md) | [teams](teams.md) | [tools](tools.md) | [ui](ui.md) | [util](util.md)

---

## Usage Pattern

```lua
function client.tick()
    spectateTick(GetAllPlayers())
end

function client.draw()
    spectateDraw()
end

function client.render(dt)
    spectateRender(dt)
end
```

---

## Functions

### [API] spectateTick(playerList)
*(client)* Update spectate state. Call once per frame.

- Enables/disables spectate when local player dies/respawns
- Tracks attacker on local player kill
- Maintains valid spectatable player list

| Param | Type | Description |
|---|---|---|
| `playerList` | table | Player IDs eligible for spectating |

```lua
spectateTick({})               -- local player only
spectateTick(GetAllPlayers())  -- all players
spectateTick(teamsGetLocalTeamPlayers()) -- team-only
```

---

### [API] spectateDraw()
*(client)* Draw spectate HUD. Call from UI/draw loop.

- Cycles players: LMB = next, RMB = previous
- Draws spectated player name label

---

### [API] spectateRender(dt)
*(client)* Update and render spectator camera. Call once per frame from render loop.

- Handles map disable/blend
- Smooth camera interpolation to spectate position
- Raycast-based occlusion avoidance
- Draws outlines on spectated player/vehicle and attacker

| Param | Type | Description |
|---|---|---|
| `dt` | number | Delta time in seconds |
