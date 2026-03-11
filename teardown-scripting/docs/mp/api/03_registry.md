# Registry

The Teardown engine uses a global key/value-pair registry for inter-script communication and persistent data.

- Hierarchical node structure; parent nodes can also hold values.
- Value types: float, int, bool, string (auto-converted on access).
- Node names: `[a-z0-9._-]` only.

### Reserved Keys

| Key | Description |
| --- | --- |
| options | reserved for game settings (write protected from mods) |
| game | reserved for the game engine internals (see documentation) |
| savegame | used for persistent game data (write protected for mods) |
| savegame.mod | used for persistent mod data. Use only alphanumeric character for key name. |
| level | not reserved, but recommended for level specific entries and script communication |

## Functions

### [API] ClearKey(key)
- **Args:**
  - `key` *(string)* — Registry key to clear
```lua
function init()
	--If the registry looks like this:
	--	score
	--		levels
	--			level1 = 5
	--			level2 = 4

	ClearKey("score.levels")

	--Afterwards, the registry will look like this:
	--	score
end
```

### [API] children = ListKeys(parent)
- **Args:**
  - `parent` *(string)* — The parent registry key
- **Returns:**
  - `children` *(table)* — Indexed table of strings with child keys
```lua
--If the registry looks like this:
--	game
--		tool
--			steroid
--			rifle
--			...

function init()
	local list = ListKeys("game.tool")
	for i=1, #list do
		DebugPrint(list[i])
	end
end

--This will output:
--steroid
--rifle
-- ...
```

### [API] exists = HasKey(key)
- **Args:**
  - `key` *(string)* — Registry key
- **Returns:**
  - `exists` *(boolean)* — True if key exists
```lua
function init()
	DebugPrint(HasKey("score.levels"))
	DebugPrint(HasKey("game.tool.rifle"))
end
```

### [API] SetInt(key, value, [sync])
- **Args:**
  - `key` *(string)* — Registry key
  - `value` *(number)* — Desired value
  - `sync` *(boolean, optional)* — Synchronize to clients
```lua
function init()
	SetInt("score.levels.level1", 4)
	DebugPrint(GetInt("score.levels.level1"))
end
```

### [API] value = GetInt(key)
- **Args:**
  - `key` *(string)* — Registry key
- **Returns:**
  - `value` *(number)* — Integer value of registry node or zero if not found
```lua
function init()
	SetInt("score.levels.level1", 4)
	DebugPrint(GetInt("score.levels.level1"))
end
```

### [API] SetFloat(key, value, [sync])
- **Args:**
  - `key` *(string)* — Registry key
  - `value` *(number)* — Desired value
  - `sync` *(boolean, optional)* — Synchronize to clients
```lua
function init()
	SetFloat("level.time", 22.3)
	DebugPrint(GetFloat("level.time"))
end
```

### [API] value = GetFloat(key)
- **Args:**
  - `key` *(string)* — Registry key
- **Returns:**
  - `value` *(number)* — Float value of registry node or zero if not found
```lua
function init()
	SetFloat("level.time", 22.3)
	DebugPrint(GetFloat("level.time"))
end
```

### [API] SetBool(key, value, [sync])
- **Args:**
  - `key` *(string)* — Registry key
  - `value` *(boolean)* — Desired value
  - `sync` *(boolean, optional)* — Synchronize to clients
```lua
function init()
	SetBool("level.robots.enabled", true)
	DebugPrint(GetBool("level.robots.enabled"))
end
```

### [API] value = GetBool(key)
- **Args:**
  - `key` *(string)* — Registry key
- **Returns:**
  - `value` *(boolean)* — Boolean value of registry node or false if not found
```lua
function init()
	SetBool("level.robots.enabled", true)
	DebugPrint(GetBool("level.robots.enabled"))
end
```

### [API] SetString(key, value, [sync])
- **Args:**
  - `key` *(string)* — Registry key
  - `value` *(string)* — Desired value
  - `sync` *(boolean, optional)* — Synchronize to clients
```lua
function init()
	SetString("level.name", "foo")
	DebugPrint(GetString("level.name"))
end
```

### [API] value = GetString(key)
- **Args:**
  - `key` *(string)* — Registry key
- **Returns:**
  - `value` *(string)* — String value of registry node or "" if not found
```lua
function init()
	SetString("level.name", "foo")
	DebugPrint(GetString("level.name"))
end
```

### [API] SetColor(key, r, g, b, [a])
- **Args:**
  - `key` *(string)* — Registry key
  - `r` *(number)* — Desired red channel value
  - `g` *(number)* — Desired green channel value
  - `b` *(number)* — Desired blue channel value
  - `a` *(number, optional)* — Desired alpha channel value
```lua
function init()
	SetColor("game.tool.wire.color", 1.0, 0.5, 0.3)
end
```

### [API] r, g, b, a = GetColor(key)
- **Args:**
  - `key` *(string)* — Registry key
- **Returns:**
  - `r` *(number)* — Desired red channel value
  - `g` *(number)* — Desired green channel value
  - `b` *(number)* — Desired blue channel value
  - `a` *(number)* — Desired alpha channel value
```lua
function init()
	SetColor("red", 1.0, 0.1, 0.1)
	color = GetColor("red")
	DebugPrint("RGBA: " .. color[1] .. " " .. color[2] .. " " .. color[3] .. " " .. color[4])
end
```

### [API] value = GetTranslatedStringByKey(key, [default])
- **Args:**
  - `key` *(string)* — Translation key
  - `default` *(string, optional)* — Default value
- **Returns:**
  - `value` *(string)* — Translation
```lua
function init()
	DebugPrint(GetTranslatedStringByKey("TOOL_CAMERA"))
end
```

### [API] value = HasTranslationByKey(key)
- **Args:**
  - `key` *(string)* — Translation key
- **Returns:**
  - `value` *(boolean)* — True if translation exists
```lua
function init()
	DebugPrint(HasTranslationByKey("TOOL_CAMERA"))
end
```

### [API] LoadLanguageTable(id)
- **Args:**
  - `id` *(number)* — Language id (enum)
| Id | Language |
| --- | --- |
| 0 | English |
| 1 | French |
| 2 | Spanish |
| 3 | Italian |
| 4 | German |
| 5 | Simplified Chinese |
| 6 | Japanese |
| 7 | Russian |
| 8 | Polish |
```lua
function init()
	-- loads the english localization table
	LoadLanguageTable(0)
end
```

### [API] value = GetUserNickname([id])
- **Args:**
  - `id` *(number, optional)* — User id
- **Returns:**
  - `value` *(string)* — User nickname
```lua
function init()
	DebugPrint(GetUserNickname(0))
end
```

---
**Navigation:** [_INDEX](_INDEX.md)