---
name: teardown-scripting
description: Use when a user wants to generate, debug, or modify Teardown Lua 5.1 mods or scripts for Singleplayer (sp) or Multiplayer (mp). Read the bundled markdown knowledge base to choose the correct API, callbacks, and multiplayer patterns.
license: MIT
---

# Teardown Scripting Skill

Use this skill when a user asks to generate, debug, or modify scripts for the Teardown game engine. 

## Process

1. **Information Gathering & Mode Selection**
   - **Crucial:** Teardown has two distinct branches: Singleplayer (`sp`) and Multiplayer (`mp`). Their scripts are *mutually incompatible* and their APIs have significant differences.
   - **Action required:** Always determine whether the user's mod is intended for Singleplayer or Multiplayer mode. If the user does not specify, **you must explicitly ask them** before proceeding.
   - Identify the specific Teardown gameplay mechanics, tools, or mod features the user wants to implement.
   - Start with the central index file `_INDEX.md`, then read only the mode-specific references it points to for the selected task.

2. **Implementation**
   - Write clean, performant Lua 5.1 code suitable for Teardown.
   - Strictly follow the function signatures, casing, and conventions defined in the Teardown API markdown docs (e.g., `SetFloat`, `SetString`, `GetPlayerPos`, Raycasting, etc.).
   - Ensure the appropriate callbacks are used correctly for the target mode:
     - **Singleplayer** — global functions: `init()`, `tick(dt)`, `update(dt)`, `draw()`.
     - **Multiplayer** — functions are nested inside `server` and `client` tables:
       - `server.init()`, `server.tick(dt)` — authoritative game logic (auto-replicated to clients).
       - `client.init()`, `client.tick(dt)`, `client.draw()` — UI, overlays, client-side audio/visuals.
       - There is **no** `update(dt)` in MP. Variables declared outside these tables exist locally on both sides but are **not** synchronized.
   - **Multiplayer scripts require `#version 2`** at the very top of the Lua file, and `version = 2` in the mod's `info.txt`. Without these, the MP architecture will not load.
   - For **singleplayer** mods, follow `_INDEX.md` to the SP API index and SP tips index before implementing from scratch.
   - For **multiplayer** mods, follow `_INDEX.md` to the MP API index, MP tips index, and **mplib** index before implementing game-mode logic from scratch.
   - Read only the specific section files needed for the requested mechanic, callback, or library module.

3. **Validation against Lua 5.1 Rules**
   - Teardown uses Lua 5.1. Ensure there are no instances of newer Lua syntactic sugar (like bitwise operators or goto jumps) unless supported by the engine or explicitly documented.
   - Remember Lua uses 1-based indexing arrays.
   - Use `--` for comments, not `//` or `/* */`.

## Output
Produce well-structured, commented Lua code snippets that can be directly pasted into a Teardown mod's script file. Explain any specific API choices based on the markdown documentation provided.