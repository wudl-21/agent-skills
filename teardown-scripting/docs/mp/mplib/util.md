# Module: util

Utilities for extracting spawn positions, tool spawns, and POIs from level tags, plus dynamic spawn point generation via raycasting.

**Navigation:** [_INDEX](_INDEX.md) | [countdown](countdown.md) | [eventlog](eventlog.md) | [hud](hud.md) | [spawn](spawn.md) | [spectate](spectate.md) | [stats](stats.md) | [teams](teams.md) | [tools](tools.md) | [ui](ui.md)

---

## Level Tags Reference

| Tag | Description |
|---|---|
| `"playerspawn"` | General player spawn (used when no team specified) |
| `"teamspawn"` | Team-specific spawn — tagged as `teamspawn=1`, `teamspawn=2`, etc. |
| `"ammospawn"` | Tool/ammo loot spawn — optional: `rarity=low` |
| `"pointofinterest"` | Gameplay interest marker — optional: `pointofinterest=1`, `pointofinterest=2` |

---

## Functions

### [API] utilLoadLevelPlayerSpawns([teamId])
Load spawn transforms from `"playerspawn"` / `"teamspawn"` level tags.

| Param | Type | Description |
|---|---|---|
| `teamId` | number | *(optional)* Filter by team ID |

**Returns:** `table` — List of spawn transforms.

---

### [API] utilLoadLevelToolSpawns([rarity])
Load tool spawn locations from `"ammospawn"` tags, optionally filtered by rarity string.

| Param | Type | Description |
|---|---|---|
| `rarity` | string | *(optional)* e.g. `"low"`, `"medium"`, `"high"` |

**Returns:** `table` — List of spawn transforms.

---

### [API] utilLoadLevelPoi([teamId])
Load points of interest from `"pointofinterest"` tags.

| Param | Type | Description |
|---|---|---|
| `teamId` | number | *(optional)* Filter by numeric team ID |

**Returns:** `table` — List of `TTransform` values.

---

### [API] utilGenerateSpawnPointLists(densities)
Generate multiple lists of spawn transforms, one per density value. Uses `utilGenerateSpawnPoints` internally.

| Param | Type | Description |
|---|---|---|
| `densities` | table | List of density floats, e.g. `{1.0, 0.66, 0.5}` |

**Returns:** `table` — List of lists, each a `TTransform` array for that density.

---

### [API] utilGenerateSpawnPointsDensity(density[, existingTransforms])
Generate spawn points for a single density value.

| Param | Type | Description |
|---|---|---|
| `density` | number | Density factor (e.g. `1.0` = high, `0.5` = half) |
| `existingTransforms` | table | *(optional)* Existing positions to avoid overlap |

**Returns:** `table` — List of `TTransform` values.

---

### [API] utilGenerateSpawnPoints(count[, existingTransforms])
Generate a fixed number of valid random spawn transforms using terrain raycasting.

| Param | Type | Description |
|---|---|---|
| `count` | number | Number of spawn points to generate |
| `existingTransforms` | table | *(optional)* Existing positions to avoid overlap |

**Returns:** `table` — List of `TTransform` values.
