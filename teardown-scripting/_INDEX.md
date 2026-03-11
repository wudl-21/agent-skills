
# Teardown Scripting — Central Index

Teardown has two distinct branches: **Singleplayer (`sp`)** and **Multiplayer (`mp`)**. Their scripts are mutually incompatible and their APIs differ significantly.

Always determine which mode the user's mod targets before proceeding.

## Knowledge Base Layout

All knowledge base files are stored under the `/docs/` directory:

### Singleplayer (`docs/sp/`)
- **API Index**: `docs/sp/api/_INDEX.md` — contains a per-section function count table, key constraints summary, and section descriptions. **Start navigation here.**
- **API Section Files**: `docs/sp/api/00_overview.md` through `docs/sp/api/22_ui.md` (23 files total, organized by feature area).
- **Tips Index**: `docs/sp/tips/_INDEX.md` — covers modding conventions, scripting basics, entity/input patterns, physics, UI, registry, scene queries, vector math, and sprites/sound/particles; includes quick-reference tables. **Read when implementing SP-specific patterns.**
- **Tips Files**: individual `.md` files under `docs/sp/tips/` (modding, scripting_basics, entities_and_input, vector_math, physics, ui, registry, scene_queries, sprites_sound_particles).

### Multiplayer (`docs/mp/`)
- **API Index**: `docs/mp/api/_INDEX.md` — contains a full alphabetical function index with links to individual section files. **Start navigation here.**
- **API Section Files**: `docs/mp/api/00_overview.md` through `docs/mp/api/24_ui.md` (25 files total; two additional chapters vs. SP: Events and Rig).
- **MPLib Index**: `docs/mp/mplib/_INDEX.md` — contains a module execution-context quick-reference table and a game-mode code skeleton. **Required reading** for any multiplayer mod.
- **MPLib Module Files**: individual `.md` files under `docs/mp/mplib/` (countdown, eventlog, hud, spawn, spectate, stats, teams, tools, ui, util).
- **Tips Index**: `docs/mp/tips/_INDEX.md` — quick-lookup table covering all MP topics: mod setup, client/server/shared architecture, player iteration, input handling, ServerCall/ClientCall, mplib, and more. **Start here for MP patterns.**
- **Tips Files**: `docs/mp/tips/modding.md` (mod setup, architecture, game modes, level markup), `docs/mp/tips/scripting.md` (scripting patterns, player iteration, optimization, advanced topics).

## How to Use This Knowledge Base

When given a user request, follow these steps:
1. Determine the target mode (ask the user explicitly: Singleplayer or Multiplayer?).
2. Read the corresponding **API index file** (`docs/sp/api/_INDEX.md` or `docs/mp/api/_INDEX.md`), locate the relevant section files from the index, then read their specific content.
3. Before implementing patterns or conventions, read the **Tips index** for the selected mode (`docs/sp/tips/_INDEX.md` or `docs/mp/tips/_INDEX.md`) and consult the specific tips files it points to.
4. For multiplayer mods, additionally read `docs/mp/mplib/_INDEX.md` for available mplib modules and their execution contexts.
