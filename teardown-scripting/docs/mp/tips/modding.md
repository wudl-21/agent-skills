# Teardown Multiplayer Modding Reference

> **[CONSTRAINTS]**
> - Requires Teardown multiplayer release
> - Assumes familiarity with single-player modding, the editor, and Lua scripting basics
> - All MP scripts require `#version 2` at the top; omitting it silently disables the script in MP sessions

---

## [Concept] Version 2 Mod Declaration

### info.txt requirement
```
version = 2
```
- Without this, the engine treats the mod as a traditional single-player mod.

### Script-level declaration
Each Lua script must begin with:
```lua
#version 2
```
- Without this tag, the script is **automatically and silently disabled** in multiplayer.

---

## [Concept] Client/Server/Shared Architecture

The same `.lua` file is loaded on **both the server and every client**. Different table sections run on different sides.

| Table    | Runs on          | Purpose                                                                 |
|----------|------------------|-------------------------------------------------------------------------|
| `server` | Host only        | Authoritative game logic; server-only global variables                   |
| `client` | Every client     | Rendering, HUD, UI, local effects, per-player overlays                  |
| `shared` | Server → Clients | Auto-replicated from server; **read-only on clients**; any serializable type |

- The host runs **both** server and client simultaneously.
- Variables declared **outside** server/client tables exist on both sides but are **not** synchronized.
- Only `shared` is automatically replicated.

> **[CONSTRAINTS]**
> - `shared` replicates every change over the network — keep it lean and minimize write frequency
> - Functions that modify the scene (e.g., `MakeHole`) are **server-only**; calling them from client raises a script error
> - `server` table has **no `draw` callback** — overlay rendering must be in `client`

---

## [Concept] Callback Functions

### Server-side callbacks
```lua
function server.init()       end  -- Called once at script load
function server.tick(dt)     end  -- Every frame (variable timestep)
function server.update(dt)   end  -- Fixed-timestep, ≤60 Hz; may run 0, 1, or 2× per frame
function server.postUpdate() end  -- After physics
function server.destroy()    end  -- When script/game mode stops
```

### Client-side callbacks
```lua
function client.init()       end  -- Called once at script load
function client.tick(dt)     end  -- Every frame (variable timestep)
function client.update(dt)   end  -- Fixed-timestep, ≤60 Hz
function client.postUpdate() end  -- After physics; used by animators
function client.draw()       end  -- 2D overlay rendering; Ui.* ONLY valid here
function client.render(dt)   end  -- Every frame before final rendering
function client.destroy()    end  -- When script/game mode stops
```

> **[CONSTRAINTS]**
> - `Ui.*` functions are **only valid** inside `client.draw()`
> - Scene-modifying API calls are **only valid** on the server side
> - There is **no** `update(dt)` equivalent note: `client.update(dt)` exists; but the SP `update(dt)` global is replaced by `server.update(dt)` / `client.update(dt)`

---

## [Concept] Game Modes

Two types of game modes exist in multiplayer:

| Type               | Defined in     | Played on                        |
|--------------------|----------------|----------------------------------|
| Global Game Mode   | Global Mod     | Any currently loaded level       |
| Content Game Mode  | Content Mod    | Specific map (`main.xml`) only   |

- Only **one game mode** can be active at a time.
- Switching game modes does **not** require scene restart.
- When host presses **Restart** in pause menu, active game mode remains active.

### gamemodes.txt — Global Game Mode definition
```
[My Game Mode 1]
description = "Restarts level on activation"
path = mygamemode1.lua
restart = true

[My Game Mode 2]
description = "No restart needed"
path = mygamemode2.lua
```

### gamemodes.txt — Content Game Mode definition
```
[My Game Mode 1]
description = "Description 1"
layers = gamemodelayer1, alwaysActiveLayer

[My Game Mode 2]
description = "Description 2"
layers = gamemodelayer2, alwaysActiveLayer
```

### Game Mode Lifecycle Callbacks
```lua
function server.init()    end  -- Game mode started
function server.destroy() end  -- Game mode stopped — clean up spawned objects
function client.init()    end  -- Game mode started on client
function client.destroy() end  -- Game mode stopped on client
```

### Level Transitions
- `StartLevel` from a Content Game Mode → next level starts with **same active game mode**.
- Switching to a Global Game Mode → new level restarted, loaded **without any layers**.
- Switching back to a Content Game Mode → always starts `main.xml` of that Content Mod.

> **[CONSTRAINTS]**
> - Content Game Modes always start on `main.xml`; game-mode-specific objects should be on a **separate layer**
> - Recommended practice: keep the map usable for global game modes by isolating game-mode assets to a layer

---

## [Concept] Standardized Level Markup

Teardown uses standardized Location Node tags for spawning and objectives. Community mods are **not required** to use them, but adopting them improves compatibility with other game modes.

### Hierarchy
```
multiplayer
├── ammo spawn
│   ├── low      → Location Nodes tagged "ammospawn rarity=low"
│   ├── medium   → Location Nodes tagged "ammospawn rarity=medium"
│   └── high     → Location Nodes tagged "ammospawn rarity=high"
├── player spawn
│   ├── free for all  → Location Nodes tagged "playerspawn"
│   └── team based
│       ├── team 1    → Location Nodes tagged "teamspawn=1"
│       └── team 2    → Location Nodes tagged "teamspawn=2"
├── Location Nodes tagged "pointofinterest=1"
└── Location Nodes tagged "pointofinterest=2"
```

### Tag Reference

| Tag                    | Use case                                                        |
|------------------------|-----------------------------------------------------------------|
| `playerspawn`          | FFA spawn; used by modes like Deathmatch                        |
| `teamspawn=N`          | Team-based spawn; N = team index (1, 2, 3, …)                  |
| `ammospawn rarity=low` | Gray crate spawn location                                       |
| `ammospawn rarity=medium` | Blue crate spawn location                                    |
| `ammospawn rarity=high`   | Red crate spawn location                                     |
| `pointofinterest=N`    | Team-specific objective/base location; N = team index           |

### Ammo Crate Types

| Rarity  | Crate Color     |
|---------|-----------------|
| Low     | Gray            |
| Medium  | Blue            |
| High    | Red             |
| Tool mod | Purple/Custom  |

> **[CONSTRAINTS]**
> - Each game mode decides crate-type-to-rarity mapping independently
> - A game mode may spawn high-rarity crates on low-rarity nodes — test with the correct game mode

---

## [Concept] Multiplayer Lua Library (mplib)

`mplib` is a Teardown-provided open-source Lua library for multiplayer game modes.

### Available Modules

| Module     | Functionality                                                            |
|------------|--------------------------------------------------------------------------|
| `eventlog` | In-game message log for game events                                      |
| `hud`      | Overlay graphics: world markers, damage feedback, timers, scoreboards    |
| `spawn`    | Finding and selecting player spawn points automatically                  |
| `spectate` | Watching other players                                                   |
| `stats`    | Score tracking                                                           |
| `teams`    | Team assignment UI, team colors, auto-balancing                          |
| `tools`    | Tool/ammo crate spawning, tool dropping/pickup                           |
| `util`     | General-purpose utility functions                                        |

- All built-in Teardown game modes use `mplib`.
- Modular design — use only the parts you need.
- Install: download source, place copy in mod folder.

**Links:**
- mplib docs: https://tuxedolabsorg.github.io/mplib/
- mplib source: https://github.com/tuxedolabsorg/mplib

---

## [Concept] Testing Multiplayer Mods

- From the editor **File menu**: launch two windows side-by-side (left = host, right = client).
- Cycle between windows: **N key**.
- Quit: select **Quit** from pause menu on the host window (closes both).
- For game modes: leave test windows open, press **F5** or **Restart** from host pause menu to reload/reconnect.

---

## Links

| Resource                          | URL                                                           |
|-----------------------------------|---------------------------------------------------------------|
| Teardown Web Site                 | https://www.teardowngame.com/                                 |
| Teardown Modding Information      | https://www.teardowngame.com/modding/                         |
| Teardown Lua API (Experimental)   | https://www.teardowngame.com/experimental/api.html            |
| Teardown Lua XML (Experimental)   | https://www.teardowngame.com/experimental/api.xml             |

---
*See also: [scripting.md](scripting.md)*
