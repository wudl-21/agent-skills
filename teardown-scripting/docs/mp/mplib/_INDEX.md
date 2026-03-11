# mplib — Knowledge Base Index

**mplib** is a multiplayer support library for the Teardown game engine.  
Provides shared Lua modules for multiplayer game modes: gameplay logic, HUD/UI, tool & loot behavior, player spawning, stats, syncing, and client/server coordination.

> Last updated: 2025-12-09

---

## Modules

| Module | Description | Context |
|---|---|---|
| [countdown](countdown.md) | Countdown timer that locks players during pre-match | server init/tick + client draw |
| [eventlog](eventlog.md) | Top-right event feed (kills, custom messages) | server post/tick + client draw |
| [hud](hud.md) | Full HUD system: damage, timers, scoreboard, markers, banners, setup UI | server init + client tick/draw/render |
| [spawn](spawn.md) | Player spawning, respawning, group-based spawn points, loadouts | server only |
| [spectate](spectate.md) | Third-person spectator camera when local player is dead | client only |
| [stats](stats.md) | Kill/death tracking per player | server tick + client read |
| [teams](teams.md) | Team assignment, colors, names, team selection UI | server logic + client draw |
| [tools](tools.md) | Loot crate spawning, tool drops on death, pickup handling | server only |
| [ui](ui.md) | Reusable UI primitives: buttons, panels, text, player rows/avatars | client only |
| [util](util.md) | Level tag parsing (spawns, POIs, ammo spawns) + procedural spawn generation | server/client |

---

## Execution Context Quick Reference

| Module | server.init | server.tick | client.tick | client.draw | client.render |
|---|---|---|---|---|---|
| countdown | `countdownInit` | `countdownTick` | — | `countdownDraw` | — |
| eventlog | — | `eventlogTick` | — | `eventlogDraw` | — |
| hud | `hudAddUnstuckButton` | — | `hudTick` | `hudDraw*` functions | — |
| spawn | `spawnInit`, `spawnSet*` | `spawnTick` | — | — | — |
| spectate | — | — | `spectateTick` | `spectateDraw` | `spectateRender` |
| stats | `statsInit` | `statsTick` | — | — | — |
| teams | `teamsInit`, `teamsSet*` | `teamsTick` | — | `teamsDraw` | — |
| tools | `toolsInit`, `toolsSet*` | `toolsTick` | — | — | — |

---

## Common Patterns

### Minimal multiplayer game mode skeleton
```lua
-- server
function server.init()
    spawnInit()
    statsInit()
    countdownInit(3.0)
    spawnSetDefaultLoadout({{"gun",7},{"sledge",0}})
    spawnSetSpawnTransforms(utilLoadLevelPlayerSpawns())
end

function server.tick(dt)
    if countdownTick(dt) then return end
    statsTick(nil)
    spawnTick(dt)
    eventlogTick(dt)
    toolsTick(dt)
end

-- client
function client.tick()
    hudTick(dt)
    spectateTick(GetAllPlayers())
end

function client.draw(dt)
    countdownDraw()
    eventlogDraw(dt)
    hudDrawTimer(remainingTime)
    spectateDraw()
end

function client.render(dt)
    spectateRender(dt)
end
```

### Team-based game mode additions
```lua
-- server.init
teamsInit(2)
teamsSetColors({{1,0.2,0.2},{0.2,0.4,1}})
teamsSetNames({"Red","Blue"})
teamsStart(false)  -- allow team selection UI

-- server.tick
teamsTick(dt)
if teamsIsSetup() then
    local teamsList = teamsGetPlayerTeamsList()
    spawnTick(dt, teamsList)
    statsTick(teamsList)
end

-- client.draw
teamsDraw(dt)  -- shows team picker until setup complete
```
