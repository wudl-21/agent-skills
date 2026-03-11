# Teardown Multiplayer Scripting — Consolidated Tips

Distilled from the official 10-part Multiplayer Scripting Tutorial series.

> **[CONSTRAINTS]**
> - All patterns here are for **version 2 (multiplayer) scripts only**
> - Requires `#version 2` at the top of every Lua file and `version = 2` in `info.txt`
> - Teardown uses Lua 5.1 — no bitwise operators, no `goto`, 1-based array indexing

---

## [Concept] Script Structure Skeleton

```lua
#version 2

-- Variables here exist on BOTH server and client but are NOT synchronized
-- Best practice: put server globals in server table, client globals in client table

function server.init()
    -- Runs once on server at load
end

function server.tick(dt)
    -- Runs every frame on server (variable timestep)
end

function client.init()
    -- Runs once on each client at load
end

function client.tick(dt)
    -- Runs every frame on each client
end

function client.draw()
    -- 2D overlay rendering — Ui.* only valid here
end
```

---

## [Concept] Player Iteration

All player-related API functions take a `playerId` as first argument in v2.

### Recommended: Custom iterator (requires `player.lua` include)
```lua
-- In server.init or server.tick:
#include "script/player.lua"   -- include from the script include folder

for playerId in Players() do
    -- Iterates over all currently connected players
    -- Automatically adjusts when players join/leave
end
```

### One-time per-player setup: `PlayersAdded` iterator
```lua
function server.tick(dt)
    for playerId in PlayersAdded() do
        -- Fires exactly once per player, even for players already in session at load
        -- Use this for setup that should happen once per player (e.g., enabling a tool)
    end
end
```

### Players leaving: `PlayersRemoved` iterator
```lua
for playerId in PlayersRemoved() do
    -- Fires once for each player that just left
end
```

> **[CONSTRAINTS]**
> - `server.init()` only runs once — do **not** use it for per-player setup; use `PlayersAdded` in `tick` instead
> - Player IDs are unique and stable; they do not change if other players join/leave

---

## [Concept] Player API (v2 Signatures)

All functions that previously took no player argument now accept `playerId` as first parameter:

```lua
GetPlayerHealth(playerId)           -- player health
GetPlayerTransform(playerId)        -- Transform
GetPlayerVelocity(playerId)         -- Vec
SetPlayerVelocity(playerId, vel)    -- set velocity
GetPlayerName(playerId)             -- string display name
GetLocalPlayer()                    -- returns local client's playerId (client-side only)
GetInteractShape(playerId)          -- shape player is hovering over
```

---

## [Concept] Input Handling (Multiple Players)

Wrap all input checks in a `Players()` loop on the server:

```lua
function server.tick(dt)
    for playerId in Players() do
        local shape = GetInteractShape(playerId)
        if shape == buttonShape and InputPressed("interact", playerId) then
            -- Handle interaction for this specific player
            SpawnParticles(...)
            PlaySound(...)
        end
    end
end
```

> **[CONSTRAINTS]**
> - Input functions require `playerId` argument in v2
> - Effects run on server replicate to all clients (but consume bandwidth — see Optimization)

---

## [Concept] Overlay Graphics — Client Draw

Overlay graphics and HUD **must** run in `client.draw()`:

```lua
function client.draw()
    for playerId in Players() do
        if playerId ~= GetLocalPlayer() then  -- skip drawing your own name
            local name = GetPlayerName(playerId)
            local transform = GetPlayerTransform(playerId)
            -- Use Ui.* functions to draw name above player's head
            UiTranslate(...)
            UiText(name)
        end
    end
end
```

> **[CONSTRAINTS]**
> - `Ui.*` / overlay functions are **only valid** inside `client.draw()`
> - Reading scene state is allowed in client callbacks; **modifying** scene entities is not

---

## [Concept] Shared Table (Server → Client Communication)

`shared` is auto-replicated from server to all clients. Clients see it as **read-only**.

```lua
-- SERVER: write to shared
function server.init()
    shared.scores = {}
end

function server.tick(dt)
    for playerId in Players() do
        if shared.scores[playerId] == nil then
            shared.scores[playerId] = 0
        end
        -- Increment when event occurs:
        -- shared.scores[playerId] = shared.scores[playerId] + 1
    end
end

-- CLIENT: read from shared
function client.draw()
    for playerId in Players() do
        local score = shared.scores[playerId] or 0
        UiText(GetPlayerName(playerId) .. ": " .. score)
    end
end
```

> **[CONSTRAINTS]**
> - Every write to `shared` triggers network replication — minimize change frequency
> - `shared` is **per-script** — cannot be used across different script files
> - For cross-script communication use the **registry** or **events**
> - Any serializable Lua data type can be stored

---

## [Concept] ServerCall (Client → Server Communication)

Used when a client needs to notify or pass data to the server (e.g., UI button press):

```lua
-- CLIENT SIDE: call server function by name
function client.draw()
    if UiButton("Ready") then
        ServerCall("OnPlayerReady", GetLocalPlayer())  -- non-blocking; may arrive several frames later
    end
end

-- SERVER SIDE: receive the call
function OnPlayerReady(playerId)
    ready[playerId] = true
end
```

> **[CONSTRAINTS]**
> - `ServerCall` is **non-blocking**: the call may arrive several frames later
> - All server calls arrive in the **same order** they were issued — no reordering
> - Cannot return values — it is a **one-way** channel only
> - Use `shared` for server→client data, `ServerCall` for client→server events

---

## [Concept] ClientCall (Server → Client Communication)

Used to trigger visual/audio effects on specific or all clients efficiently:

```lua
-- SERVER SIDE: fire a function on clients
function server.tick(dt)
    for playerId in Players() do
        if buttonPressed(playerId) then
            local pos = GetShapeWorldTransform(buttonShape).pos
            ClientCall(0, "SpawnButtonFx", pos)  -- 0 = all clients; use playerId for specific client
            -- ClientCall(playerId, "SpawnButtonFx", pos)  -- only that client
        end
    end
end

-- CLIENT SIDE: implement the function
function SpawnButtonFx(pos)
    SpawnParticle("smoke", pos, Vec(0,1,0))
    PlaySound(buttonSfx, pos)
end

function client.init()
    buttonSfx = LoadSound("button.ogg")  -- must load on client
end
```

> **[CONSTRAINTS]**
> - First argument to `ClientCall`: `0` = all clients, or specific `playerId`
> - Function is referenced **by name string** because it may not exist on the server
> - Arguments are serialized and sent over network — keep them minimal
> - Sounds and assets used in `ClientCall` targets must be loaded in `client.init()`

---

## [Concept] Optimization — Client-Side Effects

Spawning effects on the **server** replicates them to all clients over the network (costly).
Prefer spawning purely visual effects on the **client** instead:

| Effect Type                   | Recommendation         |
|-------------------------------|------------------------|
| Particles (visual only)       | **Client-side** via `ClientCall` |
| Sound effects                 | **Client-side** via `ClientCall` |
| Sprites, lines, outlines      | **Client-side**        |
| Scene-modifying actions       | **Server-side** (required) |
| Game state changes            | **Server-side** (required) |

---

## [Concept] Responsive Tool Input (Client-Side Prediction)

Network latency causes visible input lag if all tool feedback routes through the server.
Pattern: trigger **visuals/audio immediately on client** when input is detected, while the **actual effect goes through server**:

```lua
-- SERVER: authoritative action (spawning)
function server.tick(dt)
    for playerId in Players() do
        if IsToolActive(playerId) and InputPressed("usetool", playerId) then
            local fwd = GetPlayerAimTransform(playerId)
            local box = SpawnObject("box", fwd)
            SetObjectVelocity(box, TransformToWorld(fwd, Vec(0, 0, -20)))
        end
    end
end

-- CLIENT: immediate feedback (no waiting for server round-trip)
function client.tick(dt)
    if IsToolActive(GetLocalPlayer()) and InputPressed("usetool") then
        local fwd = GetPlayerAimTransform(GetLocalPlayer())
        SpawnParticle("muzzle", fwd.pos, Vec(0,0,0))
        PlaySound(shootSfx, fwd.pos)
    end
end
```

> **[CONSTRAINTS]**
> - This is **optional** — doing everything server-side still works, just with potential input lag
> - The client-side feedback (particles/sound) is purely cosmetic — actual game state is still server-authoritative
> - Requires nearly duplicate input-detection code on both server and client

---

## [Concept] Custom Tools

```lua
function server.init()
    RegisterTool("my_tool", "My Tool", "tool_model.vox")
end

function server.tick(dt)
    for playerId in PlayersAdded() do
        SetBool("tool.my_tool.enabled", true, playerId)  -- enable tool per player
    end

    for playerId in Players() do
        if GetString("tool.current", playerId) == "my_tool" then
            if InputPressed("usetool", playerId) then
                -- handle tool fire
            end
        end
    end
end
```

> **[CONSTRAINTS]**
> - Tool properties (enabled, ammo, etc.) now have **per-player API functions** instead of registry strings
> - Use `PlayersAdded` iterator for per-player tool initialization (not `server.init`)

---

## [Concept] Game Mode Patterns

```lua
-- gamemodes.txt (place in mod root folder)
-- [My Game Mode]
-- description = "..."
-- path = mygamemode.lua     ← for Global Game Modes
-- restart = true            ← optional: restart level on activation

-- For Content Game Modes, use:
-- layers = gamelayer, alwaysOn
```

```lua
-- mygamemode.lua
#version 2

function server.init()
    -- Set up: spawn ammo crates, markers, vehicles
end

function server.destroy()
    -- REQUIRED: clean up everything spawned in server.init
    -- Delete markers, crates, vehicles, etc.
end

function client.init()  end
function client.destroy()  end
```

> **[CONSTRAINTS]**
> - `server.destroy` / `client.destroy` are called when game mode is **stopped** — always clean up spawned objects
> - Levels built for global game modes provide tagged Location Nodes for spawn points; use them when appropriate
> - Test by: activating mod in Mod Manager → launch built-in map → select player count + game mode

---

## [Concept] Client-Owned Entities

Entities spawned from client-side code are **fully owned by that client** — invisible to all others:

```lua
function client.tick(dt)
    -- This box exists ONLY on this client; server and other clients are unaware
    local box = SpawnObject("debris.vox", someTransform)
    -- Can freely move, tag, or delete it from client code
    SetObjectTransform(box, newTransform)
end
```

Use cases:
- Character model animations (Teardown's own character models work this way)
- Small debris particles
- Purely cosmetic animated props

> **[CONSTRAINTS]**
> - Client-owned entities are **never synchronized** — their position can differ across clients
> - Technically: the full script is loaded everywhere; server removes `client` table, clients remove `server` table; only `shared` is synchronized
> - Variables outside server/client tables exist on both sides but are **independent copies**, not synchronized

---

## [Concept] Cross-Script Communication

| Method       | Direction           | Use case                               |
|--------------|---------------------|----------------------------------------|
| `shared`     | Server → Client     | Continuous state (scores, flags, etc.) |
| `ServerCall` | Client → Server     | Events, UI interactions                |
| `ClientCall` | Server → Client     | Trigger effects, notifications         |
| Registry     | Either side         | Cross-script data sharing              |
| Events       | Either side         | Cross-script event broadcasting        |

---

## [Concept] Typical Multiplayer UI Pattern

```lua
-- Complete ready-check UI example

-- CLIENT: draw UI and send server call on interaction
function client.draw()
    if not shared.playing then
        UiText("Press Ready to start")
        if UiButton("Ready") then
            ServerCall("OnPlayerReady", GetLocalPlayer())
        end
    end
end

-- SERVER: track ready state, update shared when all ready
local ready = {}

function OnPlayerReady(playerId)
    ready[playerId] = true
end

function server.tick(dt)
    local allReady = true
    for playerId in Players() do
        if not ready[playerId] then allReady = false end
    end
    if allReady then
        shared.playing = true
    end
end
```

---
*See also: [modding.md](modding.md)*
