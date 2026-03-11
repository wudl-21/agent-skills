# Miscellaneous

Functions of peripheral nature that doesn't fit in anywhere else

### [API] AddMapMarker(id, tag, name, category, showLabelOnMap, info, pos, color, [infoImage], [dotIcon])
- **Args:**
  - `id` *(number)* — An id to identify the marker, typically player ID or body ID.
  - `tag` *(string)* — A tag to help distinguish markers.
  - `name` *(string)* — Name of the marker.
  - `category` *(string)* — Used to group markers together in map target list.
  - `showLabelOnMap` *(bool)* — name label will be shown on map if true
  - `info` *(string)* — Additional information about the marker, displayed when selected.
  - `pos` *(Vec)* — The world position of the marker.
  - `color` *(Vec)* — The color of the marker, as a Vec table (e.g. Vec(1, 0, 0) for red)
  - `infoImage` *(string, optional)* — Path to the image to be displayed in the info section.
  - `dotIcon` *(string, optional)* — Path to the image used to represent the marker on map.
```lua
function client.tick()
	AddMapMarker(1, "bonusTarget", "Bonus Target", "One of a kind", Vec(30, 40, 50), Vec(1,0,0), "MOD/gfx/bonus_info.png", "MOD/gfx/bonus_icon.png")
end
```

### [API] id, tag = SelectedMapMarker()
- **Returns:**
  - `id` *(number)* — id of map marker that was selected this tick.
  - `tag` *(string)* — the corresponding tag.
```lua
function client.tick()
	AddMapMarker(1, "bonusTarget", "Bonus Target", "One of a kind", Vec(30, 40, 50), Vec(1,0,0), "MOD/gfx/bonus_info.png", "MOD/gfx/bonus_icon.png")

	local id, tag = SelectedMapMarker()

	if id == 1 and tag == "bonusTarget" then
		DebugPrint("You selected the Bonus Target on the map!")
	end
end
```

### [API] Shoot(origin, direction, [type], [strength], [maxDist], [playerId])
- **Args:**
  - `origin` *(TVec)* — Origin in world space as vector
  - `direction` *(TVec)* — Unit length direction as world space vector
  - `type` *(string, optional)* — Shot type, see description, default is "bullet"
  - `strength` *(number, optional)* — Strength scaling, default is 1.0
  - `maxDist` *(number, optional)* — Maximum distance, default is 100.0
  - `playerId` *(number, optional)* — Instigating player. Can be skipped for non-player shots (helicopters etc.)
```lua
function server.tick()
	Shoot(Vec(0, 10, 0), Vec(0, -1, 0), "shotgun")
end
```

### [API] Paint(origin, radius, [type], [probability])
- **Args:**
  - `origin` *(TVec)* — Origin in world space as vector
  - `radius` *(number)* — Affected radius, in range 0.0 to 5.0
  - `type` *(string, optional)* — Paint type. Can be "explosion" or "spraycan". Default is spraycan.
  - `probability` *(number, optional)* — Dithering probability between zero and one, default is 1.0
```lua
function server.tick()
	Paint(Vec(0, 2, 0), 5.0, "spraycan")
end
```

### [API] PaintRGBA(origin, radius, red, green, blue, [alpha], [probability])
- **Args:**
  - `origin` *(TVec)* — Origin in world space as vector
  - `radius` *(number)* — Affected radius, in range 0.0 to 5.0
  - `red` *(number)* — red color value, in range 0.0 to 1.0
  - `green` *(number)* — green color value, in range 0.0 to 1.0
  - `blue` *(number)* — blue color value, in range 0.0 to 1.0
  - `alpha` *(number, optional)* — alpha channel value, in range 0.0 to 1.0
  - `probability` *(number, optional)* — Dithering probability between zero and one, default is 1.0
```lua
function server.tick()
	PaintRGBA(Vec(0, 5, 0), 5.5, 1.0, 0.0, 0.0)
end
```

### [API] count = MakeHole(position, r0, [r1], [r2], [silent])
- **Args:**
  - `position` *(TVec)* — Hole center point
  - `r0` *(number)* — Hole radius for soft materials
  - `r1` *(number, optional)* — Hole radius for medium materials. May not be bigger than r0. Default zero.
  - `r2` *(number, optional)* — Hole radius for hard materials. May not be bigger than r1. Default zero.
  - `silent` *(boolean, optional)* — Make hole without playing any break sounds.
- **Returns:**
  - `count` *(number)* — Number of voxels that was cut out. This will be zero if there were no changes to any shape.
```lua
function server.init()
	MakeHole(Vec(0, 0, 0), 5.0, 1.0)
end
```

### [API] Explosion(pos, size)
- **Args:**
  - `pos` *(TVec)* — Position in world space as vector
  - `size` *(number)* — Explosion size from 0.5 to 4.0
```lua
function server.init()
	Explosion(Vec(0, 5, 0), 1)
end
```

### [API] SpawnFire(pos)
- **Args:**
  - `pos` *(TVec)* — Position in world space as vector
```lua
function server.tick()
	SpawnFire(Vec(0, 2, 0))
end
```

### [API] count = GetFireCount()
- **Returns:**
  - `count` *(number)* — Number of active fires in level
```lua
function client.tick()
	local c = GetFireCount()
	DebugPrint("Fire count " .. c)
end
```

### [API] hit, pos = QueryClosestFire(origin, maxDist)
- **Args:**
  - `origin` *(TVec)* — World space position as vector
  - `maxDist` *(number)* — Maximum search distance
- **Returns:**
  - `hit` *(boolean)* — A fire was found within search distance
  - `pos` *(TVec)* — Position of closest fire
```lua
function client.tick()
	local hit, pos = QueryClosestFire(GetPlayerTransform().pos, 5.0)
	if hit then
		--There is a fire within 5 meters to the player. Mark it with a debug cross.
		DebugCross(pos)
	end
end
```

### [API] count = QueryAabbFireCount(min, max)
- **Args:**
  - `min` *(TVec)* — Aabb minimum point
  - `max` *(TVec)* — Aabb maximum point
- **Returns:**
  - `count` *(number)* — Number of active fires in bounding box
```lua
function client.tick()
	local count = QueryAabbFireCount(Vec(0,0,0), Vec(10,10,10))
	DebugPrint(count)
end
```

### [API] count = RemoveAabbFires(min, max)
- **Args:**
  - `min` *(TVec)* — Aabb minimum point
  - `max` *(TVec)* — Aabb maximum point
- **Returns:**
  - `count` *(number)* — Number of fires removed
```lua
function server.tick()
	local removedCount= RemoveAabbFires(Vec(0,0,0), Vec(10,10,10))
	DebugPrint(removedCount)
end
```

### [API] transform = GetCameraTransform()
- **Returns:**
  - `transform` *(TTransform)* — Current camera transform
```lua
function client.tick()
	local t = GetCameraTransform()
	DebugPrint(TransformStr(t))
end
```

### [API] SetCameraTransform(transform, [fov])
- **Args:**
  - `transform` *(TTransform)* — Desired camera transform
  - `fov` *(number, optional)* — Optional horizontal field of view in degrees (default: 90)
```lua
function client.tick()
	SetCameraTransform(Transform(Vec(0, 10, 0), QuatEuler(0, 90, 0)))
end
```

### [API] RequestFirstPerson(transition)
- **Args:**
  - `transition` *(boolean)* — Use transition
```lua
function client.tick()
	if useViewFinder then
		RequestFirstPerson(true)
	end
end

function client.draw()
	if useViewFinder and !GetBool("game.thirdperson") then
		-- Draw view finder overlay
	end
end
```

### [API] RequestThirdPerson(transition)
- **Args:**
  - `transition` *(boolean)* — Use transition
```lua
function client.tick()
	if useThirdPerson then
		RequestThirdPerson(true)
	end
end
```

### [API] SetCameraOffsetTransform(transform, [stackable])
- **Args:**
  - `transform` *(TTransform)* — Desired camera offset transform
  - `stackable` *(boolean, optional)* — True if camera offset should summ up with multiple calls per tick
```lua
function client.tick()
	local tPosX = Transform(Vec(math.sin(GetTime()*3.0) * 0.2, 0, 0))
	local tPosY = Transform(Vec(0, math.cos(GetTime()*3.0) * 0.2, 0), QuatAxisAngle(Vec(0, 0, 0)))

	SetCameraOffsetTransform(tPosX, true)
	SetCameraOffsetTransform(tPosY, true)
end
```

### [API] AttachCameraTo(handle, [ignoreRotation])
- **Args:**
  - `handle` *(number)* — Body or shape handle
  - `ignoreRotation` *(boolean, optional)* — True to ignore rotation and use position only, false to use full transform
```lua
function client.tick()
	local vehicle = GetPlayerVehicle()
	if vehicle ~= 0 then
		AttachCameraTo(GetVehicleBody(vehicle))
		SetCameraOffsetTransform(Transform(Vec(1, 2, 3)))
	end
end
```

### [API] SetPivotClipBody(bodyHandle, mainShapeIdx)
- **Args:**
  - `bodyHandle` *(number)* — Handle of a body, shapes of which should be
  - `mainShapeIdx` *(number)* — Optional index of a shape among the given
```lua
local body_1 = 0
local body_2 = 0
function client.init()
	body_1 = FindBody("body_1")
	body_2 = FindBody("body_2")
end

function client.tick()
	SetPivotClipBody(body_1, 0) -- this overload should be called once and
	-- only once per frame to take effect
	SetPivotClipBody(body_2)
end
```

### [API] ShakeCamera(strength)
- **Args:**
  - `strength` *(number)* — Normalized strength of shaking
```lua
function client.tick()
	ShakeCamera(0.5)
end
```

### [API] SetCameraFov(degrees)
- **Args:**
  - `degrees` *(number)* — Horizontal field of view in degrees (10-170)
```lua
function client.tick()
	SetCameraFov(60)
end
```

### [API] SetCameraDof(distance, [amount])
- **Args:**
  - `distance` *(number)* — Depth of field distance
  - `amount` *(number, optional)* — Optional amount of blur (default 1.0)
```lua
function client.tick()
	--Set depth of field to 10 meters
	SetCameraDof(10)
end
```

### [API] SetLowHealthBlurThreshold(health)
- **Args:**
  - `health` *(number)* — health value where anything lower results in blurred vision
```lua
function client.tick()
	-- Don't show the blurry vision until the player's health drops below 0.4
	SetLowHealthBlurThreshold(0.4)
end
```

### [API] PointLight(pos, r, g, b, [intensity])
- **Args:**
  - `pos` *(TVec)* — World space light position
  - `r` *(number)* — Red
  - `g` *(number)* — Green
  - `b` *(number)* — Blue
  - `intensity` *(number, optional)* — Intensity. Default is 1.0.
```lua
function client.tick()
	--Pulsating, yellow light above world origo
	local intensity = 3 + math.sin(GetTime())
	PointLight(Vec(0, 5, 0), 1, 1, 0, intensity)
end
```

### [API] SetTimeScale(scale)
- **Args:**
  - `scale` *(number)* — Time scale 0.0 to 2.0
```lua
function server.tick()
	--Slow down time when holding down a key
	if InputDown('t', hostPlayerId) then
		SetTimeScale(0.2)
	end
end
```

### [API] SetEnvironmentDefault()
```lua
function server.init()
	SetEnvironmentDefault()
end
```

### [API] SetEnvironmentProperty(name, value0, [value1], [value2], [value3])
- **Args:**
  - `name` *(string)* — Property name
  - `value0` *(any)* — Property value (type depends on property)
  - `value1` *(any, optional)* — Extra property value (only some properties)
  - `value2` *(any, optional)* — Extra property value (only some properties)
  - `value3` *(any, optional)* — Extra property value (only some properties)
```lua
function server.init()
	SetEnvironmentDefault()
	SetEnvironmentProperty("skybox", "cloudy.dds")
	SetEnvironmentProperty("rain", 0.7)
	SetEnvironmentProperty("fogcolor", 0.5, 0.5, 0.8)
	SetEnvironmentProperty("nightlight", false)
end
```

### [API] value0, value1, value2, value3, value4 = GetEnvironmentProperty(name)
- **Args:**
  - `name` *(string)* — Property name
- **Returns:**
  - `value0` *(any)* — Property value (type depends on property)
  - `value1` *(any)* — Property value (only some properties)
  - `value2` *(any)* — Property value (only some properties)
  - `value3` *(any)* — Property value (only some properties)
  - `value4` *(any)* — Property value (only some properties)
```lua
function client.init()
	local skyboxPath = GetEnvironmentProperty("skybox")
	local rainValue = GetEnvironmentProperty("rain")
	local r,g,b = GetEnvironmentProperty("fogcolor")
	local enabled = GetEnvironmentProperty("nightlight")
	DebugPrint(skyboxPath)
	DebugPrint(rainValue)
	DebugPrint(r .. " " .. g .. " " .. b)
	DebugPrint(enabled)
end
```

### [API] SetPostProcessingDefault()
```lua
function client.tick()
	SetPostProcessingProperty("saturation", 0.4)
	SetPostProcessingProperty("colorbalance", 1.3, 1.0, 0.7)
	SetPostProcessingDefault()
end
```

### [API] SetPostProcessingProperty(name, value0, [value1], [value2])
- **Args:**
  - `name` *(string)* — Property name
  - `value0` *(number)* — Property value
  - `value1` *(number, optional)* — Extra property value (only some properties)
  - `value2` *(number, optional)* — Extra property value (only some properties)
```lua
--Sepia post processing
function client.tick()
	SetPostProcessingProperty("saturation", 0.4)
	SetPostProcessingProperty("colorbalance", 1.3, 1.0, 0.7)
end
```

### [API] value0, value1, value2 = GetPostProcessingProperty(name)
- **Args:**
  - `name` *(string)* — Property name
- **Returns:**
  - `value0` *(number)* — Property value
  - `value1` *(number)* — Property value (only some properties)
  - `value2` *(number)* — Property value (only some properties)
```lua
function client.tick()
	SetPostProcessingProperty("saturation", 0.4)
	SetPostProcessingProperty("colorbalance", 1.3, 1.0, 0.7)
	local saturation = GetPostProcessingProperty("saturation")
	local r,g,b = GetPostProcessingProperty("colorbalance")
	DebugPrint("saturation " .. saturation)
	DebugPrint("colorbalance " .. r .. " " .. g .. " " .. b)
end
```

### [API] DrawLine(p0, p1, [r], [g], [b], [a])
- **Args:**
  - `p0` *(TVec)* — World space point as vector
  - `p1` *(TVec)* — World space point as vector
  - `r` *(number, optional)* — Red
  - `g` *(number, optional)* — Green
  - `b` *(number, optional)* — Blue
  - `a` *(number, optional)* — Alpha
```lua
function server.tick()
	--Draw white debug line
	DrawLine(Vec(0, 0, 0), Vec(-10, 5, -10))

	--Draw red debug line
	DrawLine(Vec(0, 0, 0), Vec(10, 5, 10), 1, 0, 0)
end

-- Or

function client.tick()
	--Draw white debug line
	DrawLine(Vec(0, 0, 0), Vec(-10, 5, -10))

	--Draw red debug line
	DrawLine(Vec(0, 0, 0), Vec(10, 5, 10), 1, 0, 0)
end
```

### [API] DebugLine(p0, p1, [r], [g], [b], [a])
- **Args:**
  - `p0` *(TVec)* — World space point as vector
  - `p1` *(TVec)* — World space point as vector
  - `r` *(number, optional)* — Red
  - `g` *(number, optional)* — Green
  - `b` *(number, optional)* — Blue
  - `a` *(number, optional)* — Alpha
```lua
function server.tick()
	--Draw white debug line
	DebugLine(Vec(0, 0, 0), Vec(-10, 5, -10))

	--Draw red debug line
	DebugLine(Vec(0, 0, 0), Vec(10, 5, 10), 1, 0, 0)
end

-- Or

function client.tick()
	--Draw white debug line
	DebugLine(Vec(0, 0, 0), Vec(-10, 5, -10))

	--Draw red debug line
	DebugLine(Vec(0, 0, 0), Vec(10, 5, 10), 1, 0, 0)
end
```

### [API] DebugCross(p0, [r], [g], [b], [a])
- **Args:**
  - `p0` *(TVec)* — World space point as vector
  - `r` *(number, optional)* — Red
  - `g` *(number, optional)* — Green
  - `b` *(number, optional)* — Blue
  - `a` *(number, optional)* — Alpha
```lua
function server.tick()
	DebugCross(Vec(10, 5, 5))
end
-- Or
function client.tick()
	DebugCross(Vec(10, 5, 5))
end
```

### [API] DebugTransform(transform, [scale])
- **Args:**
  - `transform` *(TTransform)* — The transform
  - `scale` *(number, optional)* — Length of the axis
```lua
function server.tick()
	DebugTransform(GetPlayerCameraTransform(), 0.5)
end
-- Or
function client.tick()
	DebugTransform(GetPlayerCameraTransform(), 0.5)
end
```

### [API] DebugWatch(name, value, [lineWrapping])
- **Args:**
  - `name` *(string)* — Name
  - `value` *(any)* — Value
  - `lineWrapping` *(boolean, optional)* — True if you need to wrap Table lines. Works only with tables.
```lua
function client.tick()
	DebugWatch("Player camera transform", GetPlayerCameraTransform())

	local anyTable = {
		"teardown",
		{
			name = "Alex",
			age = 25,
			child = { name = "Lena" }
		},
		nil,
		version = "1.6.0",
		true
	}
	DebugWatch("table", anyTable);
end
```

### [API] DebugPrint(message, [lineWrapping])
- **Args:**
  - `message` *(string)* — Message to display
  - `lineWrapping` *(boolean, optional)* — True if you need to wrap Table lines. Works only with tables.
```lua
function client.init()
	DebugPrint("time")

	DebugPrint(GetPlayerCameraTransform())

	local anyTable = {
		"teardown",
		{
			name = "Alex",
			age = 25,
			child = { name = "Lena" }
		},
		nil,
		version = "1.6.0",
		true,
	}
	DebugPrint(anyTable)
end
```

### [Event] RegisterListenerTo(eventName, listenerFunction)
- **Args:**
  - `eventName` *(string)* — Event name
  - `listenerFunction` *(string)* — Listener function name
```lua
function onLangauageChanged()
	DebugPrint("langauageChanged")
end

function client.init()
	RegisterListenerTo("LanguageChanged", "onLangauageChanged")
	TriggerEvent("LanguageChanged")
end
```

### [Event] UnregisterListener(eventName, listenerFunction)
- **Args:**
  - `eventName` *(string)* — Event name
  - `listenerFunction` *(string)* — Listener function name
```lua
function onLangauageChanged()
	DebugPrint("langauageChanged")
end

function client.init()
	RegisterListenerTo("LanguageChanged", "onLangauageChanged")
	UnregisterListener("LanguageChanged", "onLangauageChanged")
	TriggerEvent("LanguageChanged")
end
```

### [Event] TriggerEvent(eventName, [args])
- **Args:**
  - `eventName` *(string)* — Event name
  - `args` *(string, optional)* — Event parameters
```lua
function onLangauageChanged()
	DebugPrint("langauageChanged")
end

function client.init()
	RegisterListenerTo("LanguageChanged", "onLangauageChanged")
	UnregisterListener("LanguageChanged", "onLangauageChanged")
	TriggerEvent("LanguageChanged")
end
```

### [API] handle = LoadHaptic(filepath)
- **Args:**
  - `filepath` *(string)* — Path to Haptic effect to play
- **Returns:**
  - `handle` *(string)* — Haptic effect handle
```lua
-- Rumble with gun Haptic effect
function client.init()
	haptic_effect = LoadHaptic("haptic/gun_fire.xml")
end

function client.tick()
	if trigHaptic then
		PlayHaptic(haptic_effect, 1)
	end
end
```

### [API] handle = CreateHaptic(leftMotorRumble, rightMotorRumble, leftTriggerRumble, rightTriggerRumble)
- **Args:**
  - `leftMotorRumble` *(number)* — Amount of rumble for left motor
  - `rightMotorRumble` *(number)* — Amount of rumble for right motor
  - `leftTriggerRumble` *(number)* — Amount of rumble for left trigger
  - `rightTriggerRumble` *(number)* — Amount of rumble for right trigger
- **Returns:**
  - `handle` *(string)* — Haptic effect handle
```lua
-- Rumble with gun Haptic effect
function client.init()
	haptic_effect = CreateHaptic(1, 1, 0, 0)
end

function client.tick()
	if trigHaptic then
		PlayHaptic(haptic_effect, 1)
	end
end
```

### [API] PlayHaptic(handle, amplitude)
- **Args:**
  - `handle` *(string)* — Handle of haptic effect
  - `amplitude` *(number)* — Amplidute used for calculation of Haptic effect.
```lua
-- Rumble with gun Haptic effect
function client.init()
	haptic_effect = LoadHaptic("haptic/gun_fire.xml")
end

function client.tick()
	if trigHaptic then
		PlayHaptic(haptic_effect, 1)
	end
end
```

### [API] PlayHapticDirectional(handle, direction, amplitude)
- **Args:**
  - `handle` *(string)* — Handle of haptic effect
  - `direction` *(TVec)* — Direction in which effect must be played
  - `amplitude` *(number)* — Amplidute used for calculation of Haptic effect.
```lua
-- Rumble with gun Haptic effect
local haptic_effect
function client.init()
	haptic_effect = LoadHaptic("haptic/gun_fire.xml")
end

function client.tick()
	if InputPressed("interact") then
		PlayHapticDirectional(haptic_effect, Vec(-1, 0, 0), 1)
	end
end
```

### [API] flag = HapticIsPlaying(handle)
- **Args:**
  - `handle` *(string)* — Handle of haptic effect
- **Returns:**
  - `flag` *(boolean)* — is current Haptic playing or not
```lua
-- Rumble infinitely
local haptic_effect
function client.init()
	haptic_effect = LoadHaptic("haptic/gun_fire.xml")
end

function client.tick()
	if not HapticIsPlaying(haptic_effect) then
		PlayHaptic(haptic_effect, 1)
	end
end
```

### [API] SetToolHaptic(id, handle, [amplitude])
- **Args:**
  - `id` *(string)* — Tool unique identifier
  - `handle` *(string)* — Handle of haptic effect
  - `amplitude` *(number, optional)* — Amplitude multiplier. Default (1.0)
```lua
function client.init()
	RegisterTool("minigun", "loc@MINIGUN", "MOD/vox/minigun.vox")
	toolHaptic = LoadHaptic("MOD/haptic/tool.xml")
	SetToolHaptic("minigun", toolHaptic)
end
```

### [API] StopHaptic(handle)
- **Args:**
  - `handle` *(string)* — Handle of haptic effect
```lua
-- Rumble infinitely
local haptic_effect
function client.init()
	haptic_effect = LoadHaptic("haptic/gun_fire.xml")
end

function client.tick()
    if InputDown("interact") then
        StopHaptic(haptic_effect)
    elseif not HapticIsPlaying(haptic_effect) then
		PlayHaptic(haptic_effect, 1)
    end
end
```

### [API] AddHeat(shape, pos, amount)
- **Args:**
  - `shape` *(number)* — Shape handle
  - `pos` *(TVec)* — World space point as vector
  - `amount` *(number)* — amount of heat
```lua
function server.tick(dt)
	if InputDown("usetool") then
		local playerCameraTransform = GetPlayerCameraTransform()
		local dir = TransformToParentVec(playerCameraTransform, Vec(0, 0, -1))

		-- Cast ray out of player camera and add heat to shape if we can find one
		local hit, dist, normal, shape = QueryRaycast(playerCameraTransform.pos, dir, 50)

		if hit then
			local hitPos = VecAdd(playerCameraTransform.pos, VecScale(dir, dist))
			AddHeat(shape, hitPos, 2 * dt)
		end

		DrawLine(VecAdd(playerCameraTransform.pos, Vec(0.5, 0, 0)), VecAdd(playerCameraTransform.pos, VecScale(dir, dist)), 1, 0, 0, 1)
	end
end
```

### [API] area = GetBoundaryArea()
- **Returns:**
  - `area` *(Number)* — Number representing the area of the boundary.
```lua
function GenerateRandomPointInLevel()
	aabbMin, aabbMax = GetBoundaryBounds()
	local x = GetRandomFloat(aabbMin[1], aabbMax[1])
	local z = GetRandomFloat(aabbMin[3], aabbMax[3])
	return x,z
end
```

### [API] min, max = GetBoundaryBounds()
- **Returns:**
  - `min` *(Vec)* — Vector representing the AABB lower bound
  - `max` *(Vec)* — Vector representing the AABB upper bound
```lua
function GenerateRandomPointInLevel()
	aabbMin, aabbMax = GetBoundaryBounds()
	local x = GetRandomFloat(aabbMin[1], aabbMax[1])
	local z = GetRandomFloat(aabbMin[3], aabbMax[3])
	return x,z
end
```

### [API] vector = GetGravity()
- **Returns:**
  - `vector` *(TVec)* — Gravity vector
```lua
function client.tick()
	DebugPrint(VecStr(GetGravity()))
end
```

### [API] SetGravity(vec)
- **Args:**
  - `vec` *(TVec)* — Gravity vector
```lua
local isMoonGravityEnabled = false

function server.tick()
	if InputPressed("g", hostPlayerId) then
		isMoonGravityEnabled = not isMoonGravityEnabled
		if isMoonGravityEnabled then
			SetGravity(Vec(0, -1.6, 0))
		else
			SetGravity(Vec(0, -10.0, 0))
		end
	end
end
```

### [API] fps = GetFps()
- **Returns:**
  - `fps` *(number)* — Frames per second
```lua
function client.tick()
	DebugWatch("fps", GetFps())
end
```

---
**Navigation:** [_INDEX](_INDEX.md)