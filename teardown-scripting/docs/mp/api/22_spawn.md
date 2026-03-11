# Spawn

Functions to spawn entities in the scene. The Spawn function can spawn prefabs from file or xml.

### [API] entities = Spawn(xml, transform, [allowStatic], [jointExisting])
- **Args:**
  - `xml` *(string)* — File name or xml string
  - `transform` *(TTransform)* — Spawn transform
  - `allowStatic` *(boolean, optional)* — Allow spawning static shapes and bodies (default false)
  - `jointExisting` *(boolean, optional)* — Allow joints to connect to existing scene geometry (default false)
- **Returns:**
  - `entities` *(table)* — Indexed table with handles to all spawned entities
```lua
function server.init()
	Spawn("MOD/prefab/mycar.xml", Transform(Vec(0, 5, 0)))
	Spawn("<voxbox size='10 10 10' prop='true' material='wood'/>", Transform(Vec(0, 10, 0)))
end
```

### [API] entities = SpawnLayer(xml, layer, transform, [allowStatic], [jointExisting])
- **Args:**
  - `xml` *(string)* — File name or xml string
  - `layer` *(string)* — Vox layer name
  - `transform` *(TTransform)* — Spawn transform
  - `allowStatic` *(boolean, optional)* — Allow spawning static shapes and bodies (default false)
  - `jointExisting` *(boolean, optional)* — Allow joints to connect to existing scene geometry (default false)
- **Returns:**
  - `entities` *(table)* — Indexed table with handles to all spawned entities
```lua
function server.init()
	Spawn("MOD/prefab/mycar.xml", "some_vox_layer", Transform(Vec(0, 5, 0)))
	Spawn("<voxbox size='10 10 10' prop='true' material='wood'/>", "some_vox_layer", Transform(Vec(0, 10, 0)))
end
```

### [API] entities = SpawnTool(id, transform, [allowStatic], [voxScale])
- **Args:**
  - `id` *(string)* — Tool ID
  - `transform` *(TTransform)* — Spawn transform
  - `allowStatic` *(boolean, optional)* — Allow spawning static shapes and bodies (default false)
  - `voxScale` *(number, optional)* — Applies a scale to voxels (default 1.0)
- **Returns:**
  - `entities` *(table)* — Indexed table with handles to all spawned entities
```lua
function server.init()
	SpawnTool("sledge", Transform(Vec(0, 5, 0)))
end
```

---
**Navigation:** [_INDEX](_INDEX.md)