# Teardown Modding Guide

> Related: [scripting_basics.md](scripting_basics.md) | [entities_and_input.md](entities_and_input.md) | [registry.md](registry.md)

## Mod Types

- **Global Mods**: Active during all gameplay when enabled. Listed on loading screen. No separate play button.
- **Content Mods**: Standalone playable environments; require `main.xml`. Show a "Play" button in Mod Manager. No effect on other content when played.

## Mod Folder Structure

```
Documents/Teardown/Mods/
└── MyMod/
    ├── info.txt          # Required: name, author, description, tags
    ├── main.lua          # Entry script (runs when mod is active)
    ├── main.xml          # Required for Content mods: level data
    ├── options.lua       # Optional: mod options screen
    ├── preview.jpg       # Required for Steam Workshop publish
    ├── spawn.txt         # Optional: spawnable asset list
    ├── id.txt            # Auto-generated on first Workshop publish (item ID)
    ├── images/
    ├── scripts/
    ├── sound/
    └── vox/
```

> **[CONSTRAINTS]**
> - File/folder names must use only Latin alphanumeric characters (a-z, 0-9, space).
> - `info.txt`, `main.xml`, `main.lua`, `options.lua`, `preview.jpg` must be in the **mod root**.

## info.txt Format

```
name = Laser Gun
author = Tuxedo Labs
description = Custom tool example mod. Laser gun that cuts through most materials
tags = Tool
```

Valid Workshop tags: `Map`, `Gameplay`, `Asset`, `Vehicle`, `Tool`

## main.xml

- Presence of `main.xml` makes the game classify the mod as a **Content mod**.
- Use `MOD/` keyword as a reference to the mod's root folder in XML paths.
  - Example: `MOD/vox/tree-pine-small.vox` → resolves to the mod's `vox/` subfolder.

## main.lua

- Runs whenever the mod is active (Global mods: always when enabled; Content mods: when played).
- For Global mods, `main.lua` in the mod root is the only option.
- For Content mods, attaching a Script node in the editor is recommended (more flexible).
- See [scripting_basics.md](scripting_basics.md) for callback functions.

## options.lua

- Triggers an **Options** button in the Mod Manager when the mod is selected.
- Must contain a `draw()` function with the options screen UI code.

## main.lua (from base game)

- To give the player their unlocked/upgraded tools from the campaign, include the base game's `main.lua` via the editor's "..." script browser.

## spawn.txt

- Lists prefab XML files to make them available in the built-in spawn menu.
- Static objects become dynamic when spawned via the spawn menu.

```txt
path/to/mycar.xml : Cars/My car
```

Format: `relative/path/from/mod/root.xml : Category/Display Name`

## Built-In Mods

- Pre-installed examples (e.g., Laser Gun). Cannot be edited directly.
- Use **"Make local copy"** in Mod Manager → saves to `Documents/Teardown/mods/` for editing.

## Steam Workshop

- Access via **"Manage subscribed..."** in Mod Manager.
- Subscribe → mod auto-added to the "Subscribed" list. Can use directly or make a local copy.
- Publish: select mod → click "Publish". Uses `info.txt` content + `preview.jpg` as thumbnail.
- First publish creates `id.txt` (Workshop item ID). Subsequent publishes → button reads "Publish update".

## Default Content Replacement

- Replicate segments of the game's `data/` folder structure inside the mod folder to override defaults.
- Example: place `data/vox/pipebomb.vox` in your mod → replaces default pipebomb model.
- Audio override: game uses `.tde` (encrypted `.ogg`). Add an `.ogg` file with the same base name in the same data path to override.
  - Engine loads `.ogg` before `.tde` when both names match.

## Custom Tools

```lua
-- Register a custom tool and enable it in player inventory
RegisterTool("lasergun", "Laser Gun", "MOD/vox/lasergun.vox")
SetBool("game.tool.lasergun.enabled", true)
```

- See the Laser Gun built-in mod for a complete example.

## Spawnable Assets (spawn.txt)

- Once exported as a prefab XML, list it in `spawn.txt`:

```txt
path/to/mycar.xml : Cars/My car
```

- Path is relative to the mod root folder.
- Note: static objects are converted to dynamic when spawned through the built-in spawn menu.

## Multi-Scene Mods

```lua
-- Load another level at runtime
StartLevel("level2", "MOD/mymodlevel2.xml")
```

- `main.xml` is the entry level. Additional scenes are separate XML files built in the editor.
- `StartLevel()` switches to a different level from within the running mod session.
