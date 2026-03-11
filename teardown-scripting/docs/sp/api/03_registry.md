# Teardown SP API — Registry
> **Version:** 1.7.0 | **Mode:** Singleplayer | **Language:** Lua 5.1

The Teardown engine uses a global key/value-pair registry that scripts can read and write. The engine exposes a lot of internal information through the registry, but it can also be used as way for scripts to communicate with each other.

The registry is a hierarchical node structure and can store a value in each node (parent nodes can also have a value). The values can be of type floating point number, integer, boolean or string, but all types are automatically converted if another type is requested. Some registry nodes are reserved and used for special purposes.

Registry node names may only contain the characters a-z, numbers 0-9, dot, dash and underscore.

**Reserved Registry Keys**

| Key | Description |
|-----|-------------|
| `options` | reserved for game settings (write protected from mods) |
| `game` | reserved for the game engine internals (see documentation) |
| `savegame` | used for persistent game data (write protected for mods) |
| `savegame.mod` | used for persistent mod data. Use only alphanumeric character for key name. |
| `level` | not reserved, but recommended for level specific entries and script communication |

> **[CONSTRAINTS]**
> - Registry node names may only contain: `a-z`, `0-9`, `.`, `-`, `_`
> - `options` and `savegame` keys are read-only for mods
> - `savegame.mod` is available for persistent mod data

---

### [API] ClearKey

```lua
ClearKey(key)
```

Remove registry node, including all child nodes.

**Arguments:**

- `key` *(string)* — Registry key to clear

**Example:**

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

---

### [API] ListKeys

```lua
children = ListKeys(parent)
```

List all child keys of a registry node.

**Arguments:**

- `parent` *(string)* — The parent registry key

**Example:**

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

---

### [API] HasKey

```lua
exists = HasKey(key)
```

Returns true if the registry contains a certain key

**Arguments:**

- `key` *(string)* — Registry key

**Example:**

```lua
function init()
	DebugPrint(HasKey("score.levels"))
	DebugPrint(HasKey("game.tool.rifle"))
end
```

---

### [API] SetInt

```lua
SetInt(key, value)
```

**Arguments:**

- `key` *(string)* — Registry key

- `value` *(number)* — Desired value

**Example:**

```lua
function init()
	SetInt("score.levels.level1", 4)
	DebugPrint(GetInt("score.levels.level1"))
end
```

---

### [API] GetInt

```lua
value = GetInt(key)
```

**Arguments:**

- `key` *(string)* — Registry key

**Example:**

```lua
function init()
	SetInt("score.levels.level1", 4)
	DebugPrint(GetInt("score.levels.level1"))
end
```

---

### [API] SetFloat

```lua
SetFloat(key, value)
```

**Arguments:**

- `key` *(string)* — Registry key

- `value` *(number)* — Desired value

**Example:**

```lua
function init()
	SetFloat("level.time", 22.3)
	DebugPrint(GetFloat("level.time"))
end
```

---

### [API] GetFloat

```lua
value = GetFloat(key)
```

**Arguments:**

- `key` *(string)* — Registry key

**Example:**

```lua
function init()
	SetFloat("level.time", 22.3)
	DebugPrint(GetFloat("level.time"))
end
```

---

### [API] SetBool

```lua
SetBool(key, value)
```

**Arguments:**

- `key` *(string)* — Registry key

- `value` *(boolean)* — Desired value

**Example:**

```lua
function init()
	SetBool("level.robots.enabled", true)
	DebugPrint(GetBool("level.robots.enabled"))
end
```

---

### [API] GetBool

```lua
value = GetBool(key)
```

**Arguments:**

- `key` *(string)* — Registry key

**Example:**

```lua
function init()
	SetBool("level.robots.enabled", true)
	DebugPrint(GetBool("level.robots.enabled"))
end
```

---

### [API] SetString

```lua
SetString(key, value)
```

**Arguments:**

- `key` *(string)* — Registry key

- `value` *(string)* — Desired value

**Example:**

```lua
function init()
	SetString("level.name", "foo")
	DebugPrint(GetString("level.name"))
end
```

---

### [API] GetString

```lua
value = GetString(key)
```

**Arguments:**

- `key` *(string)* — Registry key

**Example:**

```lua
function init()
	SetString("level.name", "foo")
	DebugPrint(GetString("level.name"))
end
```

---

### [API] GetEventCount

```lua
value = GetEventCount(type)
```

**Arguments:**

- `type` *(string)* — Event type

**Example:**

```lua
local count = GetEventCount("playerdead")
for i=1, count do
	local id, attacker = GetEvent("playerdead", i)
end
```

---

### [API] GetEvent

```lua
returnValues = GetEvent(type, index)
```

**Arguments:**

- `type` *(string)* — Event type

- `index` *(number)* — Event index (starting with one)

**Example:**

```lua
local count = GetEventCount("playerdead")
for i=1, count do
	local id, attacker = GetEvent("playerdead", i)
end
```

---

### [API] SetColor

```lua
SetColor(key, r, g, b, [a])
```

Sets the color registry key value

**Arguments:**

- `key` *(string)* — Registry key

- `r` *(number)* — Desired red channel value

- `g` *(number)* — Desired green channel value

- `b` *(number)* — Desired blue channel value

- `a` *(number, optional)* — Desired alpha channel value

**Example:**

```lua
function init()
	SetColor("game.tool.wire.color", 1.0, 0.5, 0.3)
end
```

---

### [API] GetColor

```lua
r, g, b, a = GetColor(key)
```

Returns the color registry key value

**Arguments:**

- `key` *(string)* — Registry key

**Example:**

```lua
function init()
	SetColor("red", 1.0, 0.1, 0.1)
	color = GetColor("red")
	DebugPrint("RGBA: " .. color[1] .. " " .. color[2] .. " " .. color[3] .. " " .. color[4])
end
```

---

### [API] GetTranslatedStringByKey

```lua
value = GetTranslatedStringByKey(key, [default])
```

Returns the translation for the specified key from the translation table. If the key is not found returns the default value

**Arguments:**

- `key` *(string)* — Translation key

- `default` *(string, optional)* — Default value

**Example:**

```lua
function init()
	DebugPrint(GetTranslatedStringByKey("TOOL_CAMERA"))
end
```

---

### [API] HasTranslationByKey

```lua
value = HasTranslationByKey(key)
```

Checks that translation for specified key exists

**Arguments:**

- `key` *(string)* — Translation key

**Example:**

```lua
function init()
	DebugPrint(HasTranslationByKey("TOOL_CAMERA"))
end
```

---

### [API] LoadLanguageTable

```lua
LoadLanguageTable(id)
```

Loads the language table for specified language id for further localization. Possible id values are: Id Language 0 English1 French2 Spanish3 Italian4 German5 Simplified Chinese6 Japenese7 Russian8 Polish

**Arguments:**

- `id` *(number)* — Language id (enum)

**Example:**

```lua
function init()
	-- loads the english localization table
	LoadLanguageTable(0) 
end
```

---

### [API] GetUserNickname

```lua
value = GetUserNickname([id])
```

Returns the user nickname with the specified id. If id is not specified, returns nickname for user with id '0'

**Arguments:**

- `id` *(number, optional)* — User id

**Example:**

```lua
function init()
	DebugPrint(GetUserNickname(0))
end
```

---


---
**Navigation:**
[Teardown scripting](00_overview.md) | [Parameters](01_parameters.md) | [Script control](02_script_control.md) | **Registry** | [Vector math](04_vector_math.md) | [Entity](05_entity.md) | [Body](06_body.md) | [Shape](07_shape.md) | [Location](08_location.md) | [Joint](09_joint.md) | [Animation](10_animation.md) | [Light](11_light.md) | [Trigger](12_trigger.md) | [Screen](13_screen.md) | [Vehicle](14_vehicle.md) | [Player](15_player.md) | [Sound](16_sound.md) | [Sprite](17_sprite.md) | [Scene queries](18_scene_queries.md) | [Particles](19_particles.md) | [Spawning](20_spawning.md) | [Miscellaneous](21_miscellaneous.md) | [User Interface](22_ui.md)