# Bodies, Shapes & Joints

> Related: [entities_and_input.md](entities_and_input.md) | [scene_queries.md](scene_queries.md) | [vector_math.md](vector_math.md)

## Hierarchy

```
Body (dynamic / static)
└── Shape(s)
    └── Voxel geometry
```

- A **body** is the physics rigid body; it can contain multiple **shapes**.
- Use `FindShape(tag)` to get a shape handle, then navigate up/down the hierarchy as needed.

## Navigating the Hierarchy

```lua
local targetShape = FindShape("target")
local parentBody  = GetShapeBody(targetShape)           -- shape → parent body
local allShapes   = GetBodyShapes(parentBody)           -- body → array of shapes

DebugPrint(#allShapes)   -- e.g. "2" if body has disc + target shape
```

## Setting Body Motion

```lua
-- Method 1: One-time impulse (decelerates from friction/physics)
SetBodyAngularVelocity(bodyHandle, Vec(0, 1, 0), angularSpeed)

-- Method 2: Force every tick (overrides physics — unrealistic, no resistance)
function tick(dt)
    SetBodyAngularVelocity(bodyHandle, Vec(0, 1, 0), angularSpeed)
end

-- Method 3 (Recommended for joints): Joint motor — physics-solver driven
local jointHandle = FindJoint("hinge")
SetJointMotor(jointHandle, targetVelocity, maxStrength)
```

> **[CONSTRAINTS]**
> - Forcing `SetBodyAngularVelocity` every frame bypasses physics interaction — grabbing the object has no effect.
> - Joint motors respect physics constraints (mass, friction, other forces) and provide realistic interactive motion.
> - `SetJointMotor` works on both **hinge** joints (angular) and **prismatic** joints (linear).

## Checking Shape State

```lua
if ShapeBroken(targetShape) then
    SetJointMotor(jointHandle, 0, 0)  -- stop motor: velocity=0, strength=0
end
```

## Hinge Joint — Full Spinning Example

```lua
local jointHandle
local targetShape

function init()
    jointHandle = FindJoint("hinge")
    targetShape = FindShape("target")
    SetJointMotor(jointHandle, 2.0, 100)  -- 2 rad/s, max torque 100
end

function tick(dt)
    if ShapeBroken(targetShape) then
        SetJointMotor(jointHandle, 0, 0)
    end
end
```

## Deleting Entities

```lua
Delete(handle)  -- removes entity (body, shape, etc.) from the scene immediately
```

## Shape Destruction Behavior

> **[CONSTRAINTS]**
> - When a shape is partially destroyed, the **largest fragment** retains the original handle.
> - Smaller fragments become **new shapes** with new handles (cannot be pre-tracked).
> - Small shapes/bodies are **auto-garbage-collected** by the engine to conserve resources.
> - All original shapes and bodies from level load are **guaranteed to persist** until the session ends (unless explicitly deleted with `Delete()`).
> - No guarantees on shape identity after partial destruction of complex objects.
