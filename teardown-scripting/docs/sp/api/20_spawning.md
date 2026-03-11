# Teardown SP API — Spawning
> **Version:** 1.7.0 | **Mode:** Singleplayer | **Language:** Lua 5.1

The spawn API can be used to add entities into the existing scenes. You can spawn existing prefab XML files or generate XML and pass it in as a lua string.

---

### [API] Spawn

```lua
entities = Spawn(xml, transform, [allowStatic], [jointExisting])
```

The first argument can be either a prefab XML file in your mod folder or a string with XML content. It is also possible to spawn prefabs from other mods, by using the mod id followed by colon, followed by the prefab path. Spawning prefabs from other mods should be used with causion since the referenced mod might not be installed.

**Arguments:**

- `xml` *(string)* — File name or xml string

- `transform` *(TTransform)* — Spawn transform

- `allowStatic` *(boolean, optional)* — Allow spawning static shapes and bodies (default false)

- `jointExisting` *(boolean, optional)* — Allow joints to connect to existing scene geometry (default false)

**Example:**

```lua
function init()
	Spawn("MOD/prefab/mycar.xml", Transform(Vec(0, 5, 0)))
	Spawn("<voxbox size='10 10 10' prop='true' material='wood'/>", Transform(Vec(0, 10, 0)))
end
```

---

### [API] SpawnLayer

```lua
entities = SpawnLayer(xml, layer, transform, [allowStatic], [jointExisting])
```

Same functionality as Spawn(), except using a specific layer in the vox-file

**Arguments:**

- `xml` *(string)* — File name or xml string

- `layer` *(string)* — Vox layer name

- `transform` *(TTransform)* — Spawn transform

- `allowStatic` *(boolean, optional)* — Allow spawning static shapes and bodies (default false)

- `jointExisting` *(boolean, optional)* — Allow joints to connect to existing scene geometry (default false)

**Example:**

```lua
function init()
	Spawn("MOD/prefab/mycar.xml", "some_vox_layer", Transform(Vec(0, 5, 0)))
	Spawn("<voxbox size='10 10 10' prop='true' material='wood'/>", "some_vox_layer", Transform(Vec(0, 10, 0)))
end
```

---


---
**Navigation:**
[Teardown scripting](00_overview.md) | [Parameters](01_parameters.md) | [Script control](02_script_control.md) | [Registry](03_registry.md) | [Vector math](04_vector_math.md) | [Entity](05_entity.md) | [Body](06_body.md) | [Shape](07_shape.md) | [Location](08_location.md) | [Joint](09_joint.md) | [Animation](10_animation.md) | [Light](11_light.md) | [Trigger](12_trigger.md) | [Screen](13_screen.md) | [Vehicle](14_vehicle.md) | [Player](15_player.md) | [Sound](16_sound.md) | [Sprite](17_sprite.md) | [Scene queries](18_scene_queries.md) | [Particles](19_particles.md) | **Spawning** | [Miscellaneous](21_miscellaneous.md) | [User Interface](22_ui.md)