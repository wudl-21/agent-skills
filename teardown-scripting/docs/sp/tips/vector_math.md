# Vector Math

> Related: [scene_queries.md](scene_queries.md) | [physics.md](physics.md) | [entities_and_input.md](entities_and_input.md)

## Overview

Three core types — all represented as standard Lua tables:

| Type          | Lua Representation       | Index Layout               |
|---------------|--------------------------|----------------------------|
| **Vec**        | `{x, y, z}`             | [1]=x, [2]=y, [3]=z        |
| **Quat**       | `{x, y, z, w}`          | [1]=x, [2]=y, [3]=z, [4]=w |
| **Transform**  | `{pos=Vec, rot=Quat}`   | position + rotation frame  |

> **[CONSTRAINTS]**
> - Lua is **1-indexed**: `v[1]` = x, `v[2]` = y, `v[3]` = z (not `v.x` / `v.y` / `v.z`).
> - Assigning a vector (`a = b`) creates a **reference**, not a copy. Use `VecCopy(v)` to get an independent copy.
> - Same applies to quaternions (`QuatCopy`) and transforms (`TransformCopy`).

## Constructors

```lua
local v = Vec(1, 0, 0)                        -- direction vector
local q = QuatAxisAngle(Vec(0, 1, 0), 90)     -- 90° rotation around Y axis
local t = Transform(Vec(0, 5, 0), q)          -- position + rotation
```

## Copying

```lua
local a = Vec(1, 2, 3)
local b = VecCopy(a)    -- independent copy; mutations to b don't affect a
local c = QuatCopy(q)
local d = TransformCopy(t)
```

## Transform Space Conversions

```lua
-- World space → Local space relative to a parent transform
local localPos = TransformToLocalPoint(parentTransform, worldPos)

-- Local space → World space
local worldPos = TransformToParentPoint(parentTransform, localPos)
```

### Pattern: Attach a World Position to a Moving Object (Computed Once)

```lua
local localOffset   -- vehicle-local position of the marker, computed once in init

function init()
    local markerTransform  = GetLocationTransform(FindLocation("marker"))
    local vehicleTransform = GetBodyTransform(FindBody("vehicle"))
    -- Convert world marker position to vehicle-local space
    localOffset = TransformToLocalPoint(vehicleTransform, markerTransform.pos)
end

function tick(dt)
    -- Get current vehicle transform every frame (it moves)
    local vehicleTransform = GetBodyTransform(vehicleHandle)
    -- Convert vehicle-local offset back to world space
    local worldPos = TransformToParentPoint(vehicleTransform, localOffset)
    DebugCross(worldPos)  -- must call every frame; only visible for 1 frame
end
```

## Common Vector Functions

```lua
VecAdd(a, b)          -- a + b
VecSub(a, b)          -- a - b
VecScale(v, s)        -- v * scalar s
VecLength(v)          -- magnitude |v|
VecNormalize(v)       -- v / |v|  (unit vector)
VecDot(a, b)          -- dot product
VecCross(a, b)        -- cross product
VecLerp(a, b, t)      -- linear interpolation (t: 0.0–1.0)
VecReflect(v, n)      -- reflect v over surface normal n
```

## Transform / Quaternion Functions

```lua
GetLocationTransform(handle)      -- returns Transform for a location entity
GetBodyTransform(handle)          -- returns Transform for a body entity
SetBodyTransform(handle, t)       -- set body transform
TransformToLocalPoint(t, p)       -- world point → local (inverse transform)
TransformToParentPoint(t, p)      -- local point → world (forward transform)
TransformToLocalVector(t, v)      -- world direction → local direction
TransformToParentVector(t, v)     -- local direction → world direction
QuatLookAt(from, to)              -- rotation facing from→to direction
QuatEuler(x, y, z)               -- rotation from Euler angles (degrees)
```

## Debug Visualization

```lua
DebugCross(worldPos)                  -- draws ✕ marker at position for 1 frame
DrawLine(from, to, r, g, b, a)        -- draws debug line for 1 frame
```

> **[CONSTRAINTS]**
> - `DebugCross` and `DrawLine` only render for a single frame — must be called every frame from `tick()`.
