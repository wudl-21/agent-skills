# Rig

Rig functions.
A rig contains a set of named transforms often used by the player script to set IK-targets, but it can be used as a general transform container as well.
Transforms are stored internally as local transforms relative the rig transform.
A rig can itself be a child to a body(using "relative_parent" tag) or vehicle(automatically relative to vehicle)

### [API] handle = FindRig([tag], [global])
- **Args:**
  - `tag` *(string, optional)* — Tag name
  - `global` *(boolean, optional)* — Search in entire scene
- **Returns:**
  - `handle` *(number)* — Handle to first rig with specified tag or zero if not found
```lua
function client.init()
	local rig = FindRig("myrig")
end
```

### [API] transform = GetRigWorldTransform(rig)
- **Args:**
  - `rig` *(number)* — Rig handle
- **Returns:**
  - `transform` *(TTransform)* — World transform, nil if rig is missing
```lua
    local t = GetRigWorldTransform(rig)
```

### [API] SetRigWorldTransform(rig, transform)
- **Args:**
  - `rig` *(number)* — Rig handle
  - `transform` *(TTransform)* — New world transform
```lua
    SetRigWorldTransform(rig, Transform(...))
```

### [API] transform = GetRigLocationWorldTransform(rig, name)
- **Args:**
  - `rig` *(number)* — Rig handle
  - `name` *(string)* — Name of location
- **Returns:**
  - `transform` *(TTransform)* — World transform, nil if rig is missing or location is missing
```lua
local foot_t = GetRigLocationWorldTransform(rigid, "ik_foot_l")
```

### [API] SetRigLocationWorldTransform(rig, name, transform)
- **Args:**
  - `rig` *(number)* — Rig handle
  - `name` *(string)* — Name of location
  - `transform` *(TTransform)* — New world transform
```lua
    SetRigLocationWorldTransform(rig, "some_location_name", Transform(...))
```

### [API] transform = GetRigLocationLocalTransform(rig, name)
- **Args:**
  - `rig` *(number)* — Rig handle
  - `name` *(string)* — Name of location
- **Returns:**
  - `transform` *(TTransform)* — Local transform, nil if rig is missing or location is missing
```lua
local t = GetRigLocationLocalTransform(rigid, "some_location_name")
```

### [API] SetRigLocationLocalTransform(rig, name, transform)
- **Args:**
  - `rig` *(number)* — Rig handle
  - `name` *(string)* — Name of location
  - `transform` *(TTransform)* — New world transform
```lua
    local someBody = FindBody("bodyname")
    SetPlayerRigTransform(someBody, GetBodyTransform(someBody))
```

---
**Navigation:** [_INDEX](_INDEX.md)