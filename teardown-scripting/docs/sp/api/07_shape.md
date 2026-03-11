# Teardown SP API — Shape
> **Version:** 1.7.0 | **Mode:** Singleplayer | **Language:** Lua 5.1

A shape is a voxel object and always owned by a body. A single body may contain multiple shapes. The transform of shape is expressed in the parent body coordinate system.

---

### [API] FindShape

```lua
handle = FindShape([tag], [global])
```

**Arguments:**

- `tag` *(string, optional)* — Tag name

- `global` *(boolean, optional)* — Search in entire scene

**Example:**

```lua
local target = 0
local escape = 0
function init()
	--Search for a shape tagged "mybox" in script scope
	target = FindShape("mybox")

	--Search for a shape tagged "laserturret" in entire scene
	escape = FindShape("laserturret", true)
end

function tick()
	DebugCross(GetShapeWorldTransform(target).pos)
	DebugCross(GetShapeWorldTransform(escape).pos)
end
```

---

### [API] FindShapes

```lua
list = FindShapes([tag], [global])
```

**Arguments:**

- `tag` *(string, optional)* — Tag name

- `global` *(boolean, optional)* — Search in entire scene

**Example:**

```lua
local shapes = {}
function init()
	--Search for shapes tagged "body"
	shapes = FindShapes("body", true)
end

function tick()
	for i=1, #shapes do
		local shape = shapes[i]
		DebugCross(GetShapeWorldTransform(shape).pos)
	end
end
```

---

### [API] GetShapeLocalTransform

```lua
transform = GetShapeLocalTransform(handle)
```

**Arguments:**

- `handle` *(number)* — Shape handle

**Example:**

```lua
local shape = 0
function init()
	shape = FindShape("shape")
end

function tick()
	--Shape transform in body local space
	local shapeTransform = GetShapeLocalTransform(shape)

	--Body transform in world space
	local bodyTransform = GetBodyTransform(GetShapeBody(shape))

	--Shape transform in world space
	local worldTranform = TransformToParentTransform(bodyTransform, shapeTransform)

	DebugCross(worldTranform)
end
```

---

### [API] SetShapeLocalTransform

```lua
SetShapeLocalTransform(handle, transform)
```

**Arguments:**

- `handle` *(number)* — Shape handle

- `transform` *(TTransform)* — Shape transform in body space

**Example:**

```lua
local shape = 0
function init()
	shape = FindShape("shape")
	local transform = Transform(Vec(0, 1, 0), QuatEuler(0, 90, 0))
	SetShapeLocalTransform(shape, transform)
end

function tick()
	--Shape transform in body local space
	local shapeTransform = GetShapeLocalTransform(shape)

	--Body transform in world space
	local bodyTransform = GetBodyTransform(GetShapeBody(shape))

	--Shape transform in world space
	local worldTranform = TransformToParentTransform(bodyTransform, shapeTransform)

	DebugCross(worldTranform)
end
```

---

### [API] GetShapeWorldTransform

```lua
transform = GetShapeWorldTransform(handle)
```

This is a convenience function, transforming the shape out of body space

**Arguments:**

- `handle` *(number)* — Shape handle

**Example:**

```lua
--GetShapeWorldTransform is equivalent to
--local shapeTransform = GetShapeLocalTransform(shape)
--local bodyTransform = GetBodyTransform(GetShapeBody(shape))
--worldTranform = TransformToParentTransform(bodyTransform, shapeTransform)

local shape = 0
function init()
	shape = FindShape("shape", true)
end

function tick()
	DebugCross(GetShapeWorldTransform(shape).pos)
end
```

---

### [API] GetShapeBody

```lua
handle = GetShapeBody(handle)
```

Get handle to the body this shape is owned by. A shape is always owned by a body, but can be transfered to a new body during destruction.

**Arguments:**

- `handle` *(number)* — Shape handle

**Example:**

```lua
local body = 0
function init()
	body = GetShapeBody(FindShape("shape", true), true)
end

function tick()
	DebugCross(GetBodyCenterOfMass(body))
end
```

---

### [API] GetShapeJoints

```lua
list = GetShapeJoints(shape)
```

**Arguments:**

- `shape` *(number)* — Shape handle

**Example:**

```lua
local shape = 0
function init()
	shape = FindShape("shape", true)

	local hinges = GetShapeJoints(shape)
	for i=1, #hinges do
		local joint = hinges[i]
		DebugPrint(joint)
	end
end
```

---

### [API] GetShapeLights

```lua
list = GetShapeLights(shape)
```

**Arguments:**

- `shape` *(number)* — Shape handle

**Example:**

```lua
local shape = 0
function init()
	shape = FindShape("shape", true)

	local light = GetShapeLights(shape)
	for i=1, #light do
		DebugPrint(light[i])
	end
end
```

---

### [API] GetShapeBounds

```lua
min, max = GetShapeBounds(handle)
```

Return the world space, axis-aligned bounding box for a shape.

**Arguments:**

- `handle` *(number)* — Shape handle

**Example:**

```lua
local shape = 0
function init()
	shape = FindShape("shape", true)

	local min, max = GetShapeBounds(shape)
	local boundsSize = VecSub(max, min)
	local center = VecLerp(min, max, 0.5)

	DebugPrint(VecStr(boundsSize) .. " " .. VecStr(center))
end
```

---

### [API] SetShapeEmissiveScale

```lua
SetShapeEmissiveScale(handle, scale)
```

Scale emissiveness for shape. If the shape has light sources attached, their intensity will be scaled by the same amount.

**Arguments:**

- `handle` *(number)* — Shape handle

- `scale` *(number)* — Scale factor for emissiveness

**Example:**

```lua
local shape = 0
function init()
	shape = FindShape("shape", true)

	--Pulsate emissiveness and light intensity for shape
	local scale = math.sin(GetTime())*0.5 + 0.5
	SetShapeEmissiveScale(shape, scale)
end
```

---

### [API] SetShapeDensity

```lua
SetShapeDensity(handle, density)
```

Change the material density of the shape.

**Arguments:**

- `handle` *(number)* — Shape handle

- `density` *(number)* — New density for the shape

**Example:**

```lua
local shape = 0
function init()
	shape = FindShape("shape", true)

	local density = 10.0
	SetShapeDensity(shape, density)
end
```

---

### [API] GetShapeMaterialAtPosition

```lua
type, r, g, b, a, entry = GetShapeMaterialAtPosition(handle, pos, [includeUnphysical])
```

Return material properties for a particular voxel

**Arguments:**

- `handle` *(number)* — Shape handle

- `pos` *(TVec)* — Position in world space

- `includeUnphysical` *(boolean, optional)* — Include unphysical voxels in the search. Default false.

**Example:**

```lua
local shape = 0
function init()
	shape = FindShape("shape", true)
end

function tick()
	local pos = GetCameraTransform().pos
	local dir = Vec(0, 0, 1)
	local hit, dist, normal, shape = QueryRaycast(pos, dir, 10)
	if hit then
		local hitPoint = VecAdd(pos, VecScale(dir, dist))
		local mat = GetShapeMaterialAtPosition(shape, hitPoint)
		DebugPrint("Raycast hit voxel made out of " .. mat)
	end
	DebugLine(pos, VecAdd(pos, VecScale(dir, 10)))
end
```

---

### [API] GetShapeMaterialAtIndex

```lua
type, r, g, b, a, entry = GetShapeMaterialAtIndex(handle, x, y, z)
```

Return material properties for a particular voxel in the voxel grid indexed by integer values. The first index is zero (not one, as opposed to a lot of lua related things)

**Arguments:**

- `handle` *(number)* — Shape handle

- `x` *(number)* — X integer coordinate

- `y` *(number)* — Y integer coordinate

- `z` *(number)* — Z integer coordinate

**Example:**

```lua
local shape = 0
function init()
	shape = FindShape("shape", true)
	local mat = GetShapeMaterialAtIndex(shape, 0, 0, 0)
	DebugPrint("The voxel is of material: " .. mat)
end
```

---

### [API] GetShapeSize

```lua
xsize, ysize, zsize, scale = GetShapeSize(handle)
```

Return the size of a shape in voxels

**Arguments:**

- `handle` *(number)* — Shape handle

**Example:**

```lua
local shape = 0
function init()
	shape = FindShape("shape", true)
	local x, y, z = GetShapeSize(shape)
	DebugPrint("Shape size: " .. x .. ";" .. y .. ";" .. z)
end
```

---

### [API] GetShapeVoxelCount

```lua
count = GetShapeVoxelCount(handle)
```

Return the number of voxels in a shape, not including empty space

**Arguments:**

- `handle` *(number)* — Shape handle

**Example:**

```lua
local shape = 0
function init()
	shape = FindShape("shape", true)
	local voxelCount = GetShapeVoxelCount(shape)
	DebugPrint(voxelCount)
end
```

---

### [API] IsShapeVisible

```lua
visible = IsShapeVisible(handle, maxDist, [rejectTransparent])
```

This function does a very rudimetary check and will only return true if the object is visible within 74 degrees of the camera's forward direction, and only tests line-of-sight visibility for the corners and center of the bounding box.

**Arguments:**

- `handle` *(number)* — Shape handle

- `maxDist` *(number)* — Maximum visible distance

- `rejectTransparent` *(boolean, optional)* — See through transparent materials. Default false.

**Example:**

```lua
local shape = 0
function init()
	shape = FindShape("shape", true)
end

function tick()
	if IsShapeVisible(shape, 25) then
		DebugPrint("Shape is visible")
	else
		DebugPrint("Shape is not visible")
	end
end
```

---

### [API] IsShapeBroken

```lua
broken = IsShapeBroken(handle)
```

Determine if shape has been broken. Note that a shape can be transfered to another body during destruction, but might still not be considered broken if all voxels are intact.

**Arguments:**

- `handle` *(number)* — Shape handle

**Example:**

```lua
local shape = 0
function init()
	shape = FindShape("shape", true)
end

function tick()
	DebugPrint("Is shape broken: " .. tostring(IsShapeBroken(shape)))
end
```

---

### [API] DrawShapeOutline

```lua
DrawShapeOutline(handle, [r], [g], [b], a)
```

Render next frame with an outline around specified shape. If no color is given, a white outline will be drawn.

**Arguments:**

- `handle` *(number)* — Shape handle

- `r` *(number, optional)* — Red

- `g` *(number, optional)* — Green

- `b` *(number, optional)* — Blue

- `a` *(number)* — Alpha

**Example:**

```lua
local shape = 0
function init()
	shape = FindShape("shape", true)
end

function tick()
	if InputDown("interact") then
		--Draw white outline at 50% transparency
		DrawShapeOutline(shape, 0.5)
	else
		--Draw green outline, fully opaque
		DrawShapeOutline(shape, 0, 1, 0, 1)
	end
end
```

---

### [API] DrawShapeHighlight

```lua
DrawShapeHighlight(handle, amount)
```

Flash the appearance of a shape when rendering this frame.

**Arguments:**

- `handle` *(number)* — Shape handle

- `amount` *(number)* — Amount

**Example:**

```lua
local shape = 0
function init()
	shape = FindShape("shape", true)
end

function tick()
	if InputDown("interact") then
		DrawShapeHighlight(shape, 0.5)
	end
end
```

---

### [API] SetShapeCollisionFilter

```lua
SetShapeCollisionFilter(handle, layer, mask)
```

This is used to filter out collisions with other shapes. Each shape can be given a layer bitmask (8 bits, 0-255) along with a mask (also 8 bits). The layer of one object must be in the mask of the other object and vice versa for the collision to be valid. The default layer for all objects is 1 and the default mask is 255 (collide with all layers).

**Arguments:**

- `handle` *(number)* — Shape handle

- `layer` *(number)* — Layer bits (0-255)

- `mask` *(number)* — Mask bits (0-255)

**Example:**

```lua
local shapeA = 0
local shapeB = 0
local shapeC = 0
local shapeD = 0
function init()
	shapeA = FindShape("shapeA")
	shapeB = FindShape("shapeB")
	shapeC = FindShape("shapeC")
	shapeD = FindShape("shapeD")
	--This will put shapes a and b in layer 2 and disable collisions with
	--object shapes in layers 2, preventing any collisions between the two.
	SetShapeCollisionFilter(shapeA, 2, 255-2)
	SetShapeCollisionFilter(shapeB, 2, 255-2)

	--This will put shapes c and d in layer 4 and allow collisions with other
	--shapes in layer 4, but ignore all other collisions with the rest of the world.
	SetShapeCollisionFilter(shapeC, 4, 4)
	SetShapeCollisionFilter(shapeD, 4, 4)
end
```

---

### [API] GetShapeCollisionFilter

```lua
layer, mask = GetShapeCollisionFilter(handle)
```

Returns the current layer/mask settings of the shape

**Arguments:**

- `handle` *(number)* — Shape handle

**Example:**

```lua
function init()
	local shape = FindShape("some_shape")
	local layer, mask = GetShapeCollisionFilter(shape)
end
```

---

### [API] CreateShape

```lua
newShape = CreateShape(body, transform, refShape)
```

Create new, empty shape on existing body using the palette of a reference shape. The reference shape can be any existing shape in the scene or an external vox file. The size of the new shape will be 1x1x1.

**Arguments:**

- `body` *(number)* — Body handle

- `transform` *(TTransform)* — Shape transform in body space

- `refShape` *(number)* — Handle to reference shape or path to vox file

**Example:**

```lua
function tick()
	if InputPressed("interact") then
		local t = Transform(Vec(0, 5, 0), QuatEuler(0, 0, 0))
		local handle = CreateShape(FindBody("shape", true), t, FindShape("shape", true))
		DebugPrint(handle)
	end
end
```

---

### [API] ClearShape

```lua
ClearShape(shape)
```

Fill a voxel shape with zeroes, thus removing all voxels.

**Arguments:**

- `shape` *(number)* — Shape handle

**Example:**

```lua
function init()
	ClearShape(FindShape("shape", true))
end
```

---

### [API] ResizeShape

```lua
resized, offset = ResizeShape(shape, xmi, ymi, zmi, xma, yma, zma)
```

Resize an existing shape. The new coordinates are expressed in the existing shape coordinate frame, so you can provide negative values. The existing content is preserved, but may be cropped if needed. The local shape transform will be moved automatically with an offset vector to preserve the original content in body space. This offset vector is returned in shape local space.

**Arguments:**

- `shape` *(number)* — Shape handle

- `xmi` *(number)* — Lower X coordinate

- `ymi` *(number)* — Lower Y coordinate

- `zmi` *(number)* — Lower Z coordinate

- `xma` *(number)* — Upper X coordinate

- `yma` *(number)* — Upper Y coordinate

- `zma` *(number)* — Upper Z coordinate

**Example:**

```lua
function init()
	ResizeShape(FindShape("shape", true), -5, 0, -5, 5, 5, 5)
end
```

---

### [API] SetShapeBody

```lua
SetShapeBody(shape, body, [transform])
```

Move existing shape to a new body, optionally providing a new local transform.

**Arguments:**

- `shape` *(number)* — Shape handle

- `body` *(number)* — Body handle

- `transform` *(TTransform, optional)* — New local shape transform. Default is existing local transform.

**Example:**

```lua
function init()
	SetShapeBody(FindShape("shape", true), FindBody("custombody", true), true)
end
```

---

### [API] CopyShapeContent

```lua
CopyShapeContent(src, dst)
```

Copy voxel content from source shape to destination shape. If destination shape has a different size, it will be resized to match the source shape.

**Arguments:**

- `src` *(number)* — Source shape handle

- `dst` *(number)* — Destination shape handle

**Example:**

```lua
function init()
	CopyShapeContent(FindShape("shape", true), FindShape("shape2", true))
end
```

---

### [API] CopyShapePalette

```lua
CopyShapePalette(src, dst)
```

Copy the palette from source shape to destination shape.

**Arguments:**

- `src` *(number)* — Source shape handle

- `dst` *(number)* — Destination shape handle

**Example:**

```lua
function init()
	CopyShapePalette(FindShape("shape", true), FindShape("shape2", true))
end
```

---

### [API] GetShapePalette

```lua
entries = GetShapePalette(shape)
```

Return list of material entries, each entry is a material index that can be provided to GetShapeMaterial or used as brush for populating a shape.

**Arguments:**

- `shape` *(number)* — Shape handle

**Example:**

```lua
function init()
	local palette = GetShapePalette(FindShape("shape2", true))
	for i = 1, #palette do
		DebugPrint(palette[i])
	end
end
```

---

### [API] GetShapeMaterial

```lua
type, red, green, blue, alpha, reflectivity, shininess, metallic, emissive = GetShapeMaterial(shape, entry)
```

Return material properties for specific matirial entry.

**Arguments:**

- `shape` *(number)* — Shape handle

- `entry` *(number)* — Material entry

**Example:**

```lua
function init()
	local type, r, g, b, a, reflectivity, shininess, metallic, emissive = GetShapeMaterial(FindShape("shape2", true), 1)
	DebugPrint(type)
end
```

---

### [API] SetBrush

```lua
SetBrush(type, size, index, [object])
```

Set material index to be used for following calls to DrawShapeLine and DrawShapeBox and ExtrudeShape. An optional brush vox file and subobject can be used and provided instead of material index, in which case the content of the brush will be used and repeated. Use material index zero to remove of voxels.

**Arguments:**

- `type` *(string)* — One of "sphere", "cube" or "noise"

- `size` *(number)* — Size of brush in voxels (must be in range 1 to 16)

- `index` *(or)* — Material index or path to brush vox file

- `object` *(string, optional)* — Optional object in brush vox file if brush vox file is used

**Example:**

```lua
function init()
	SetBrush("sphere", 3, 3)
end
```

---

### [API] DrawShapeLine

```lua
DrawShapeLine(shape, x0, y0, z0, x1, y1, z1, [paint], [noOverwrite])
```

Draw voxelized line between (x0,y0,z0) and (x1,y1,z1) into shape using the material set up with SetBrush. Paint mode will only change material of existing voxels (where the current material index is non-zero). noOverwrite mode will only fill in voxels if the space isn't already accupied by another shape in the scene.

**Arguments:**

- `shape` *(number)* — Handle to shape

- `x0` *(number)* — Start X coordinate

- `y0` *(number)* — Start Y coordinate

- `z0` *(number)* — Start Z coordinate

- `x1` *(number)* — End X coordinate

- `y1` *(number)* — End Y coordinate

- `z1` *(number)* — End Z coordinate

- `paint` *(boolean, optional)* — Paint mode. Default is false.

- `noOverwrite` *(boolean, optional)* — Only fill in voxels if space isn't already occupied. Default is false.

**Example:**

```lua
function init()
	SetBrush("sphere", 3, 1)
	DrawShapeLine(FindShape("shape"), 0, 0, 0, 10, 50, 5, false, true)
end
```

---

### [API] DrawShapeBox

```lua
DrawShapeBox(shape, x0, y0, z0, x1, y1, z1)
```

Draw box between (x0,y0,z0) and (x1,y1,z1) into shape using the material set up with SetBrush.

**Arguments:**

- `shape` *(number)* — Handle to shape

- `x0` *(number)* — Start X coordinate

- `y0` *(number)* — Start Y coordinate

- `z0` *(number)* — Start Z coordinate

- `x1` *(number)* — End X coordinate

- `y1` *(number)* — End Y coordinate

- `z1` *(number)* — End Z coordinate

**Example:**

```lua
function init()
	SetBrush("sphere", 3, 4)
	DrawShapeBox(FindShape("shape", true), 0, 0, 0, 10, 50, 5)
end
```

---

### [API] ExtrudeShape

```lua
ExtrudeShape(shape, x, y, z, dx, dy, dz, steps, mode)
```

Extrude region of shape. The extruded region will be filled in with the material set up with SetBrush. The mode parameter sepcifies how the region is determined. Exact mode selects region of voxels that exactly match the input voxel at input coordinate. Material mode selects region that has the same material type as the input voxel. Geometry mode selects any connected voxel in the same plane as the input voxel.

**Arguments:**

- `shape` *(number)* — Handle to shape

- `x` *(number)* — X coordinate to extrude

- `y` *(number)* — Y coordinate to extrude

- `z` *(number)* — Z coordinate to extrude

- `dx` *(number)* — X component of extrude direction, should be -1, 0 or 1

- `dy` *(number)* — Y component of extrude direction, should be -1, 0 or 1

- `dz` *(number)* — Z component of extrude direction, should be -1, 0 or 1

- `steps` *(number)* — Length of extrusion in voxels

- `mode` *(string)* — Extrusion mode, one of "exact", "material", "geometry". Default is "exact"

**Example:**

```lua
local shape = 0
function init()
	SetBrush("sphere", 3, 4)
	shape = FindShape("shape")
	ExtrudeShape(shape, 0, 5, 0, -1, 0, 0, 50, "exact")
end
```

---

### [API] TrimShape

```lua
offset = TrimShape(shape)
```

Trim away empty regions of shape, thus potentially making it smaller. If the size of the shape changes, the shape will be automatically moved to preserve the shape content in body space. The offset vector for this translation is returned in shape local space.

**Arguments:**

- `shape` *(number)* — Source handle

**Example:**

```lua
local shape = 0
function init()
	shape = FindShape("shape", true)
	TrimShape(shape)
end
```

---

### [API] SplitShape

```lua
newShapes = SplitShape(shape, removeResidual)
```

Split up a shape into multiple shapes based on connectivity. If the removeResidual flag is used, small disconnected chunks will be removed during this process to reduce the number of newly created shapes.

**Arguments:**

- `shape` *(number)* — Source handle

- `removeResidual` *(boolean)* — Remove residual shapes (default false)

**Example:**

```lua
local shape = 0
function init()
	shape = FindShape("shape", true)
	SplitShape(shape, true)
end
```

---

### [API] MergeShape

```lua
shape = MergeShape(shape)
```

Try to merge shape with a nearby, matching shape. For a merge to happen, the shapes need to be aligned to the same rotation and touching. If the provided shape was merged into another shape, that shape may be resized to fit the merged content. If shape was merged, the handle to the other shape is returned, otherwise the input handle is returned.

**Arguments:**

- `shape` *(number)* — Input shape

**Example:**

```lua
local shape = 0
function init()
	shape = FindShape("shape", true)
	DebugPrint(shape)
	shape = MergeShape(shape)
	DebugPrint(shape)
end
```

---

### [API] IsShapeDisconnected

```lua
disconnected = IsShapeDisconnected(shape)
```

**Arguments:**

- `shape` *(number)* — Input shape

**Example:**

```lua
function tick()
	DebugWatch("IsShapeDisconnected", IsShapeDisconnected(FindShape("shape", true)))
end
```

---

### [API] IsStaticShapeDetached

```lua
disconnected = IsStaticShapeDetached(shape)
```

**Arguments:**

- `shape` *(number)* — Input shape

**Example:**

```lua
function tick()
	DebugWatch("IsStaticShapeDetached", IsStaticShapeDetached(FindShape("shape_glass", true)))
end
```

---

### [API] GetShapeClosestPoint

```lua
hit, point, normal = GetShapeClosestPoint(shape, origin)
```

This will return the closest point of a specific shape

**Arguments:**

- `shape` *(number)* — Shape handle

- `origin` *(TVec)* — World space point

**Example:**

```lua
local shape = 0
function init()
	shape = FindShape("shape", true)
end

function tick()
	DebugCross(Vec(1, 0, 0))
	local hit, p, n, s = GetShapeClosestPoint(shape, Vec(1, 0, 0))
	if hit then
		DebugCross(p)
	end
end
```

---

### [API] IsShapeTouching

```lua
touching = IsShapeTouching(a, b)
```

This will check if two shapes has physical overlap

**Arguments:**

- `a` *(number)* — Handle to first shape

- `b` *(number)* — Handle to second shape

**Example:**

```lua
local shapeA = 0
local shapeB = 0
function init()
	shapeA = FindShape("shape")
	shapeB = FindShape("shape2")
end

function tick()
	DebugPrint(IsShapeTouching(shapeA, shapeB))
end
```

---


---
**Navigation:**
[Teardown scripting](00_overview.md) | [Parameters](01_parameters.md) | [Script control](02_script_control.md) | [Registry](03_registry.md) | [Vector math](04_vector_math.md) | [Entity](05_entity.md) | [Body](06_body.md) | **Shape** | [Location](08_location.md) | [Joint](09_joint.md) | [Animation](10_animation.md) | [Light](11_light.md) | [Trigger](12_trigger.md) | [Screen](13_screen.md) | [Vehicle](14_vehicle.md) | [Player](15_player.md) | [Sound](16_sound.md) | [Sprite](17_sprite.md) | [Scene queries](18_scene_queries.md) | [Particles](19_particles.md) | [Spawning](20_spawning.md) | [Miscellaneous](21_miscellaneous.md) | [User Interface](22_ui.md)