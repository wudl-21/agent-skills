# Teardown Multiplayer Tips — Knowledge Base Index

Knowledge base for Teardown **multiplayer modding**. Covers mod setup, the client/server scripting architecture, game modes, level markup, and the mplib library.

## Files

| File | Contents |
|------|----------|
| [modding.md](modding.md) | Mod setup (`info.txt`, `#version 2`), client/server/shared architecture, callback reference, game mode types (`gamemodes.txt`), level markup tags, mplib overview, testing workflow |
| [scripting.md](scripting.md) | Scripting patterns: player iteration, input handling, shared table, `ServerCall`, `ClientCall`, optimization, responsive tools, game mode lifecycle, client-owned entities, cross-script communication |

## Quick Lookup

| Topic | File | Section |
|-------|------|---------|
| Enable multiplayer in a mod | modding.md | Version 2 Mod Declaration |
| `server` / `client` / `shared` tables | modding.md | Client/Server/Shared Architecture |
| All server and client callbacks | modding.md | Callback Functions |
| Define a game mode (`gamemodes.txt`) | modding.md | Game Modes |
| Level spawn point tags | modding.md | Standardized Level Markup |
| mplib modules | modding.md | Multiplayer Lua Library |
| Script skeleton (`#version 2`) | scripting.md | Script Structure Skeleton |
| Iterate over all players | scripting.md | Player Iteration |
| Per-player input handling | scripting.md | Input Handling |
| Draw overlays / HUD | scripting.md | Overlay Graphics — Client Draw |
| Share state server→client | scripting.md | Shared Table |
| Client → server event (`ServerCall`) | scripting.md | ServerCall |
| Server → client effect (`ClientCall`) | scripting.md | ClientCall |
| Reduce bandwidth / move effects to client | scripting.md | Optimization |
| Input-lag-free tool feedback | scripting.md | Responsive Tool Input |
| Custom tool registration | scripting.md | Custom Tools |
| Game mode init/destroy cleanup | scripting.md | Game Mode Patterns |
| Client-only local entities | scripting.md | Client-Owned Entities |
| UI ready-check pattern | scripting.md | Typical Multiplayer UI Pattern |

## Source Material

| # | Tutorial Video | Raw Transcript |
|---|---------------|----------------|
| 1 | Multiplayer Scripting Part 1 - Introduction | scripting/Part1.txt |
| 2 | Multiplayer Scripting Part 2 - Multiple Players | scripting/Part2.txt |
| 3 | Multiplayer Scripting Part 3 - Input handling | scripting/Part3.txt |
| 4 | Multiplayer Scripting Part 4 - Overlay graphics | scripting/Part4.txt |
| 5 | Multiplayer Scripting Part 5 - User Interfaces | scripting/Part5.txt |
| 6 | Multiplayer Scripting Part 6 - Optimization | scripting/Part6.txt |
| 7 | Multiplayer Scripting Part 7 - Custom tools | scripting/Part7.txt |
| 8 | Multiplayer Scripting Part 8 - Game modes | scripting/Part8.txt |
| 9 | Multiplayer Scripting Part 9 - Multiplayer Library - mplib | scripting/Part9.txt |
| 10 | Multiplayer Scripting Part 10 - Advanced topics | scripting/Part10.txt |
| — | Modding documentation page | modding/index.html |
