# Shape

A shape is a voxel object and always owned by a body. A single body may contain multiple shapes. The transform
of shape is expressed in the parent body coordinate system.

### [API] handle = FindShape([tag], [global])
- **Args:**
  - `tag` *(string, optional)* — Tag name
  - `global` *(boolean, optional)* — Search in entire scene
- **Returns:**
  - `handle` *(number)* — Handle to first shape with specified tag or zero if not found
```lua
local target = 0
local escape = 0
function client.init()
	--Search for a shape tagged "mybox" in script scope
	target = FindShape("mybox")

	--Search for a shape tagged "laserturret" in entire scene
	escape = FindShape("laserturret", true)
end

function client.tick()
	DebugCross(GetShapeWorldTransform(target).pos)
	DebugCross(GetShapeWorldTransform(escape).pos)
end
```

### [API] list = FindShapes([tag], [global])
- **Args:**
  - `tag` *(string, optional)* — Tag name
  - `global` *(boolean, optional)* — Search in entire scene
- **Returns:**
  - `list` *(table)* — Indexed table with handles to all shapes with specified tag
```lua
local shapes = {}
function client.init()
	--Search for shapes tagged "body"
	shapes = FindShapes("body", true)
end

function client.tick()
	for i=1, #shapes do
		local shape = shapes[i]
		DebugCross(GetShapeWorldTransform(shape).pos)
	end
end
```

### [API] transform = GetShapeLocalTransform(handle)
- **Args:**
  - `handle` *(number)* — Shape handle
- **Returns:**
  - `transform` *(TTransform)* — Return shape transform in body space
```lua
local shape = 0
function client.init()
	shape = FindShape("shape")
end

function client.tick()
	--Shape transform in body local space
	local shapeTransform = GetShapeLocalTransform(shape)

	--Body transform in world space
	local bodyTransform = GetBodyTransform(GetShapeBody(shape))

	--Shape transform in world space
	local worldTranform = TransformToParentTransform(bodyTransform, shapeTransform)

	DebugCross(worldTranform)
end
```

### [API] SetShapeLocalTransform(handle, transform)
- **Args:**
  - `handle` *(number)* — Shape handle
  - `transform` *(TTransform)* — Shape transform in body space
```lua
local shape = 0
function server.init()
	shape = FindShape("shape")
	local transform = Transform(Vec(0, 1, 0), QuatEuler(0, 90, 0))
	SetShapeLocalTransform(shape, transform)
end

function client.init()
	shape = FindShape("shape")
end

function client.tick()
	--Shape transform in body local space
	local shapeTransform = GetShapeLocalTransform(shape)

	--Body transform in world space
	local bodyTransform = GetBodyTransform(GetShapeBody(shape))

	--Shape transform in world space
	local worldTranform = TransformToParentTransform(bodyTransform, shapeTransform)

	DebugCross(worldTranform)
end
```

### [API] transform = GetShapeWorldTransform(handle)
- **Args:**
  - `handle` *(number)* — Shape handle
- **Returns:**
  - `transform` *(TTransform)* — Return shape transform in world space
```lua
--GetShapeWorldTransform is equivalent to
--local shapeTransform = GetShapeLocalTransform(shape)
--local bodyTransform = GetBodyTransform(GetShapeBody(shape))
--worldTranform = TransformToParentTransform(bodyTransform, shapeTransform)

local shape = 0
function client.init()
	shape = FindShape("shape", true)
end

function client.tick()
	DebugCross(GetShapeWorldTransform(shape).pos)
end
```

### [API] handle = GetShapeBody(handle)
- **Args:**
  - `handle` *(number)* — Shape handle
- **Returns:**
  - `handle` *(number)* — Body handle
```lua
local body = 0
function client.init()
	body = GetShapeBody(FindShape("shape", true))
end

function client.tick()
	DebugCross(GetBodyCenterOfMass(body))
end
```

### [API] list = GetShapeJoints(shape)
- **Args:**
  - `shape` *(number)* — Shape handle
- **Returns:**
  - `list` *(table)* — Indexed table with joints connected to shape
```lua
function printJoints()
	local shape = FindShape("shape", true)

	local hinges = GetShapeJoints(shape)
	for i=1, #hinges do
		local joint = hinges[i]
		DebugPrint(joint)
	end
end
```

### [API] list = GetShapeLights(shape)
- **Args:**
  - `shape` *(number)* — Shape handle
- **Returns:**
  - `list` *(table)* — Indexed table of lights owned by shape
```lua
function printLights()
	--Print all lights owned by a shape
	local shape = FindShape("shape", true)

	local light = GetShapeLights(shape)
	for i=1, #light do
		DebugPrint(light[i])
	end
end
```

### [API] min, max = GetShapeBounds(handle)
- **Args:**
  - `handle` *(number)* — Shape handle
- **Returns:**
  - `min` *(TVec)* — Vector representing the AABB lower bound
  - `max` *(TVec)* — Vector representing the AABB upper bound
```lua
function printShapeBounds()
	local shape = FindShape("shape", true)

	local min, max = GetShapeBounds(shape)
	local boundsSize = VecSub(max, min)
	local center = VecLerp(min, max, 0.5)

	DebugPrint(VecStr(boundsSize) .. " " .. VecStr(center))
end
```

### [API] SetShapeEmissiveScale(handle, scale)
- **Args:**
  - `handle` *(number)* — Shape handle
  - `scale` *(number)* — Scale factor for emissiveness
```lua
local shape = 0
function server.init()
	shape = FindShape("shape", true)

	--Pulsate emissiveness and light intensity for shape
	local scale = math.sin(GetTime())*0.5 + 0.5
	SetShapeEmissiveScale(shape, scale)
end
```

### [API] SetShapeDensity(handle, density)
- **Args:**
  - `handle` *(number)* — Shape handle
  - `density` *(number)* — New density for the shape
```lua
local shape = 0
function server.init()
	shape = FindShape("shape", true)

	local density = 10.0
	SetShapeDensity(shape, density)
end
```

### [API] type, r, g, b, a, entry = GetShapeMaterialAtPosition(handle, pos, [includeUnphysical])
- **Args:**
  - `handle` *(number)* — Shape handle
  - `pos` *(TVec)* — Position in world space
  - `includeUnphysical` *(boolean, optional)* — Include unphysical voxels in the search. Default false.
- **Returns:**
  - `type` *(string)* — Material type
  - `r` *(number)* — Red
  - `g` *(number)* — Green
  - `b` *(number)* — Blue
  - `a` *(number)* — Alpha
  - `entry` *(number)* — Palette entry for voxel (zero if empty)
```lua
local shape = 0
function client.init()
	shape = FindShape("shape", true)
end

function client.tick()
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

### [API] type, r, g, b, a, entry = GetShapeMaterialAtIndex(handle, x, y, z)
- **Args:**
  - `handle` *(number)* — Shape handle
  - `x` *(number)* — X integer coordinate
  - `y` *(number)* — Y integer coordinate
  - `z` *(number)* — Z integer coordinate
- **Returns:**
  - `type` *(string)* — Material type
  - `r` *(number)* — Red
  - `g` *(number)* — Green
  - `b` *(number)* — Blue
  - `a` *(number)* — Alpha
  - `entry` *(number)* — Palette entry for voxel (zero if empty)
```lua
local shape = 0
function client.init()
	shape = FindShape("shape", true)
	local mat = GetShapeMaterialAtIndex(shape, 0, 0, 0)
	DebugPrint("The voxel is of material: " .. mat)
end
```

### [API] xsize, ysize, zsize, scale = GetShapeSize(handle)
- **Args:**
  - `handle` *(number)* — Shape handle
- **Returns:**
  - `xsize` *(number)* — Size in voxels along x axis
  - `ysize` *(number)* — Size in voxels along y axis
  - `zsize` *(number)* — Size in voxels along z axis
  - `scale` *(number)* — The size of one voxel in meters (with default scale it is 0.1)
```lua
local shape = 0
function client.init()
	shape = FindShape("shape", true)
	local x, y, z = GetShapeSize(shape)
	DebugPrint("Shape size: " .. x .. ";" .. y .. ";" .. z)
end
```

### [API] count = GetShapeVoxelCount(handle)
- **Args:**
  - `handle` *(number)* — Shape handle
- **Returns:**
  - `count` *(number)* — Number of voxels in shape
```lua
local shape = 0
function client.init()
	shape = FindShape("shape", true)
	local voxelCount = GetShapeVoxelCount(shape)
	DebugPrint(voxelCount)
end
```

### [API] visible = IsShapeVisible(handle, maxDist, [rejectTransparent], [playerId])
- **Args:**
  - `handle` *(number)* — Shape handle
  - `maxDist` *(number)* — Maximum visible distance
  - `rejectTransparent` *(boolean, optional)* — See through transparent materials. Default false.
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server(host) player.
- **Returns:**
  - `visible` *(boolean)* — Return true if shape is visible
```lua
local shape = 0
function client.init()
	shape = FindShape("shape", true)
end

function client.tick()
	if IsShapeVisible(shape, 25) then
		DebugPrint("Shape is visible")
	else
		DebugPrint("Shape is not visible")
	end
end
```

### [API] broken = IsShapeBroken(handle)
- **Args:**
  - `handle` *(number)* — Shape handle
- **Returns:**
  - `broken` *(boolean)* — Return true if shape is broken
```lua
local shape = 0
function client.init()
	shape = FindShape("shape", true)
end

function client.tick()
	DebugPrint("Is shape broken: " .. tostring(IsShapeBroken(shape)))
end
```

### [API] DrawShapeOutline(handle, [r], [g], [b], a)
- **Args:**
  - `handle` *(number)* — Shape handle
  - `r` *(number, optional)* — Red
  - `g` *(number, optional)* — Green
  - `b` *(number, optional)* — Blue
  - `a` *(number)* — Alpha
```lua
local shape = 0
function client.init()
	shape = FindShape("shape", true)
end

function client.tick()
	if InputDown("interact") then
		--Draw white outline at 50% transparency
		DrawShapeOutline(shape, 0.5)
	else
		--Draw green outline, fully opaque
		DrawShapeOutline(shape, 0, 1, 0, 1)
	end
end
```

### [API] DrawShapeHighlight(handle, amount)
- **Args:**
  - `handle` *(number)* — Shape handle
  - `amount` *(number)* — Amount
```lua
local shape = 0
function client.init()
	shape = FindShape("shape", true)
end

function client.tick()
	if InputDown("interact") then
		DrawShapeHighlight(shape, 0.5)
	end
end
```

### [API] SetShapeCollisionFilter(handle, layer, mask)
- **Args:**
  - `handle` *(number)* — Shape handle
  - `layer` *(number)* — Layer bits (0-255)
  - `mask` *(number)* — Mask bits (0-255)
```lua
local shapeA = 0
local shapeB = 0
local shapeC = 0
local shapeD = 0
function server.init()
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

### [API] layer, mask = GetShapeCollisionFilter(handle)
- **Args:**
  - `handle` *(number)* — Shape handle
- **Returns:**
  - `layer` *(number)* — Layer bits (0-255)
  - `mask` *(number)* — Mask bits (0-255)
```lua
function server.init()
	local shape = FindShape("some_shape")
	local layer, mask = GetShapeCollisionFilter(shape)
end
```

### [API] newShape = CreateShape(body, transform, refShape)
- **Args:**
  - `body` *(number)* — Body handle
  - `transform` *(TTransform)* — Shape transform in body space
  - `refShape` *(number or string)* — Handle to reference shape or path to vox file
- **Returns:**
  - `newShape` *(number)* — Handle of new shape
```lua
server.tick()
	local players = GetAllPlayers()
	for i=1, #players do
		tickPlayer(players[i])
	end
end

function tickPlayer(playerId)
	if InputPressed("interact", playerId) then
		local t = Transform(Vec(0, 5, 0), QuatEuler(0, 0, 0))
		local handle = CreateShape(FindBody("shape", true), t, FindShape("shape", true))
		DebugPrint(handle)
	end
end
```

### [API] ClearShape(shape)
- **Args:**
  - `shape` *(number)* — Shape handle
```lua
function server.init()
	ClearShape(FindShape("shape", true))
end
```

### [API] resized, offset = ResizeShape(shape, xmi, ymi, zmi, xma, yma, zma)
- **Args:**
  - `shape` *(number)* — Shape handle
  - `xmi` *(number)* — Lower X coordinate
  - `ymi` *(number)* — Lower Y coordinate
  - `zmi` *(number)* — Lower Z coordinate
  - `xma` *(number)* — Upper X coordinate
  - `yma` *(number)* — Upper Y coordinate
  - `zma` *(number)* — Upper Z coordinate
- **Returns:**
  - `resized` *(boolean)* — Resized successfully
  - `offset` *(TVec)* — Offset vector in shape local space
```lua
function server.init()
	ResizeShape(FindShape("shape", true), -5, 0, -5, 5, 5, 5)
end
```

### [API] SetShapeBody(shape, body, [transform])
- **Args:**
  - `shape` *(number)* — Shape handle
  - `body` *(number)* — Body handle
  - `transform` *(TTransform, optional)* — New local shape transform. Default is existing local transform.
```lua
function server.init()
	SetShapeBody(FindShape("shape", true), FindBody("custombody", true), true)
end
```

### [API] CopyShapeContent(src, dst)
- **Args:**
  - `src` *(number)* — Source shape handle
  - `dst` *(number)* — Destination shape handle
```lua
function server.init()
	CopyShapeContent(FindShape("shape", true), FindShape("shape2", true))
end
```

### [API] CopyShapePalette(src, dst)
- **Args:**
  - `src` *(number)* — Source shape handle
  - `dst` *(number)* — Destination shape handle
```lua
function server.init()
	CopyShapePalette(FindShape("shape", true), FindShape("shape2", true))
end
```

### [API] entries = GetShapePalette(shape)
- **Args:**
  - `shape` *(number)* — Shape handle
- **Returns:**
  - `entries` *(table)* — Palette material entries
```lua
function server.init()
	local palette = GetShapePalette(FindShape("shape2", true))
	for i = 1, #palette do
		DebugPrint(palette[i])
	end
end
```

### [API] type, red, green, blue, alpha, reflectivity, shininess, metallic, emissive = GetShapeMaterial(shape, entry)
- **Args:**
  - `shape` *(number)* — Shape handle
  - `entry` *(number)* — Material entry
- **Returns:**
  - `type` *(string)* — Type
  - `red` *(number)* — Red value
  - `green` *(number)* — Green value
  - `blue` *(number)* — Blue value
  - `alpha` *(number)* — Alpha value
  - `reflectivity` *(number)* — Range 0 to 1
  - `shininess` *(number)* — Range 0 to 1
  - `metallic` *(number)* — Range 0 to 1
  - `emissive` *(number)* — Range 0 to 32
```lua
function client.init()
	local type, r, g, b, a, reflectivity, shininess, metallic, emissive = GetShapeMaterial(FindShape("shape2", true), 1)
	DebugPrint(type)
end
```

### [API] SetBrush(type, size, index or path, [object])
- **Args:**
  - `type` *(string)* — One of "sphere", "cube" or "noise"
  - `size` *(number)* — Size of brush in voxels (must be in range 1 to 16)
  - `index or path` *(number or string)* — Material index or path to brush vox file
  - `object` *(string, optional)* — Optional object in brush vox file if brush vox file is used
```lua
function server.init()
	SetBrush("sphere", 3, 3)
end
```

### [API] DrawShapeLine(shape, x0, y0, z0, x1, y1, z1, [paint], [noOverwrite])
- **Args:**
  - `shape` *(number)* — Handle to shape
  - `x0` *(number)* — Start X coordinate
  - `y0` *(number)* — Start Y coordinate
  - `z0` *(number)* — Start Z coordinate
  - `x1` *(number)* — End X coordinate
  - `y1` *(number)* — End Y coordinate
  - `z1` *(number)* — End Z coordinate
  - `paint` *(boolean, optional)* — Paint mode. Default is false.
  - `noOverwrite` *(boolean, optional)* — Only fill in voxels if space isn't already occupied. Default is false.
```lua
function server.init()
	SetBrush("sphere", 3, 1)
	DrawShapeLine(FindShape("shape"), 0, 0, 0, 10, 50, 5, false, true)
end
```

### [API] DrawShapeBox(shape, x0, y0, z0, x1, y1, z1)
- **Args:**
  - `shape` *(number)* — Handle to shape
  - `x0` *(number)* — Start X coordinate
  - `y0` *(number)* — Start Y coordinate
  - `z0` *(number)* — Start Z coordinate
  - `x1` *(number)* — End X coordinate
  - `y1` *(number)* — End Y coordinate
  - `z1` *(number)* — End Z coordinate
```lua
function server.init()
	SetBrush("sphere", 3, 4)
	DrawShapeBox(FindShape("shape", true), 0, 0, 0, 10, 50, 5)
end
```

### [API] ExtrudeShape(shape, x, y, z, dx, dy, dz, steps, mode)
- **Args:**
  - `shape` *(number)* — Handle to shape
  - `x` *(number)* — X coordinate to extrude
  - `y` *(number)* — Y coordinate to extrude
  - `z` *(number)* — Z coordinate to extrude
  - `dx` *(number)* — X component of extrude direction, should be -1, 0 or 1
  - `dy` *(number)* — Y component of extrude direction, should be -1, 0 or 1
  - `dz` *(number)* — Z component of extrude direction, should be -1, 0 or 1
  - `steps` *(number)* — Length of extrusion in voxels
  - `mode` *(string)* — Extrusion mode, one of "exact", "material", "geometry". Default is "exact"
```lua
local shape = 0
function server.init()
	SetBrush("sphere", 3, 4)
	shape = FindShape("shape")
	ExtrudeShape(shape, 0, 5, 0, -1, 0, 0, 50, "exact")
end
```

### [API] offset = TrimShape(shape)
- **Args:**
  - `shape` *(number)* — Source handle
- **Returns:**
  - `offset` *(TVec)* — Offset vector in shape local space
```lua
local shape = 0
function server.init()
	shape = FindShape("shape", true)
	TrimShape(shape)
end
```

### [API] newShapes = SplitShape(shape, removeResidual)
- **Args:**
  - `shape` *(number)* — Source handle
  - `removeResidual` *(boolean)* — Remove residual shapes (default false)
- **Returns:**
  - `newShapes` *(table)* — List of shape handles created
```lua
local shape = 0
function server.init()
	shape = FindShape("shape", true)
	SplitShape(shape, true)
end
```

### [API] shape = MergeShape(shape)
- **Args:**
  - `shape` *(number)* — Input shape
- **Returns:**
  - `shape` *(number)* — Shape handle after merge
```lua
local shape = 0
function server.init()
	shape = FindShape("shape", true)
	DebugPrint(shape)
	shape = MergeShape(shape)
	DebugPrint(shape)
end
```

### [API] disconnected = IsShapeDisconnected(shape)
- **Args:**
  - `shape` *(number)* — Input shape
- **Returns:**
  - `disconnected` *(boolean)* — True if shape disconnected (has detached parts)
```lua
function client.tick()
	DebugWatch("IsShapeDisconnected", IsShapeDisconnected(FindShape("shape", true)))
end
```

### [API] disconnected = IsStaticShapeDetached(shape)
- **Args:**
  - `shape` *(number)* — Input shape
- **Returns:**
  - `disconnected` *(boolean)* — True if static shape has detached parts
```lua
function client.tick()
	DebugWatch("IsStaticShapeDetached", IsStaticShapeDetached(FindShape("shape_glass", true)))
end
```

### [API] hit, point, normal = GetShapeClosestPoint(shape, origin)
- **Args:**
  - `shape` *(number)* — Shape handle
  - `origin` *(TVec)* — World space point
- **Returns:**
  - `hit` *(boolean)* — True if a point was found
  - `point` *(TVec)* — World space closest point
  - `normal` *(TVec)* — World space normal at closest point
```lua
local shape = 0
function client.init()
	shape = FindShape("shape", true)
end

function client.tick()
	DebugCross(Vec(1, 0, 0))
	local hit, p, n, s = GetShapeClosestPoint(shape, Vec(1, 0, 0))
	if hit then
		DebugCross(p)
	end
end
```

### [API] touching = IsShapeTouching(a, b)
- **Args:**
  - `a` *(number)* — Handle to first shape
  - `b` *(number)* — Handle to second shape
- **Returns:**
  - `touching` *(boolean)* — True is shapes a and b are touching each other
```lua
local shapeA = 0
local shapeB = 0
function client.init()
	shapeA = FindShape("shape")
	shapeB = FindShape("shape2")
end

function client.tick()
	DebugPrint(IsShapeTouching(shapeA, shapeB))
end
```

---
**Navigation:** [_INDEX](_INDEX.md)