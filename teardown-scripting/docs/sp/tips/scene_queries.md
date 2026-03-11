# Scene Queries: Triggers, Raycasting & Volume Queries

> Related: [entities_and_input.md](entities_and_input.md) | [physics.md](physics.md) | [vector_math.md](vector_math.md)

## Triggers

- Set up in the editor (box, sphere, or extruded polygon shape).
- Query at runtime whether a body/shape is inside:

```lua
local crateBody   = FindBody("crate")
local triggerArea = FindTrigger("zone")

function tick(dt)
    if IsBodyInTrigger(triggerArea, crateBody) then
        Delete(crateBody)
    end
end
```

## Raycasting

```lua
local hit, dist, normal, shape = QueryRaycast(
    origin,     -- Vec: ray start position (world space)
    direction,  -- Vec: normalized direction
    maxLength   -- float: max ray distance in meters (use 1000 if unsure)
)
-- hit:    boolean — true if ray hit something
-- dist:   float   — distance to the hit point (meters)
-- normal: Vec     — surface normal at hit point
-- shape:  handle  — shape that was hit
```

### Excluding Own Body (Avoid Self-Hit)

The ray origin is often *inside* a shape (e.g., the laser device). Use `QueryRejectBody` to exclude it from the **immediately following** query:

```lua
QueryRejectBody(laserBodyHandle)  -- excludes from the NEXT QueryRaycast only
local hit, dist, normal = QueryRaycast(origin, direction, 1000)
```

> **[CONSTRAINTS]**
> - `QueryRejectBody` only affects the **immediately following** query call.
> - Without it, a ray starting inside a shape reports `dist = 0` (hits itself immediately).

## Reflected Laser Beam Pattern

```lua
function tick(dt)
    local t   = GetBodyTransform(laserBodyHandle)
    local dir = TransformToParentVector(t, Vec(0, 0, 1))  -- forward in world space

    local origin = t.pos

    for i = 1, 5 do  -- max 5 reflections
        QueryRejectBody(laserBodyHandle)
        local hit, dist, normal = QueryRaycast(origin, dir, 1000)

        local endPos = VecAdd(origin, VecScale(dir, hit and (dist - 0.01) or 1000))
        DrawLine(origin, endPos, 1, 0, 0, 1)  -- draw beam segment

        if not hit then break end

        -- Reflect direction over surface normal and advance origin
        dir    = VecReflect(dir, normal)
        origin = endPos
    end
end
```

> **[CONSTRAINTS]**
> - Subtract a small offset (`~0.01`) from hit distance when computing the reflection origin, otherwise the next ray starts *inside* the surface and returns dist=0.
> - Each loop iteration requires its own `QueryRejectBody` call before `QueryRaycast`.

## Volume Queries

```lua
QueryAabb(minVec, maxVec)             -- returns handles within an axis-aligned bounding box
QuerySphere(center, radius)           -- returns handles within a sphere volume
QueryClosestPoint(bodyHandle, pos)    -- returns closest point on a body's surface to a world position
```

## Filtering Options

Scene queries support various filter flags — see API docs for full list:
- Filter by body/shape type (dynamic, static, etc.)
- Tag-based filtering
- Layer masks
