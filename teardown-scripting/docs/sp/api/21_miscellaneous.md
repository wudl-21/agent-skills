# Teardown SP API — Miscellaneous
> **Version:** 1.7.0 | **Mode:** Singleplayer | **Language:** Lua 5.1

Functions of peripheral nature that doesn't fit in anywhere else

---

### [API] Shoot

```lua
Shoot(origin, direction, [type], [strength], [maxDist])
```

Fire projectile. Type can be one of "bullet", "rocket", "gun" or "shotgun". For backwards compatilbility, type also accept a number, where 1 is same as "rocket" and anything else "bullet" Note that this function will only spawn the projectile, not make any sound Also note that "bullet" and "rocket" are the only projectiles that can hurt the player.

**Arguments:**

- `origin` *(TVec)* — Origin in world space as vector

- `direction` *(TVec)* — Unit length direction as world space vector

- `type` *(string, optional)* — Shot type, see description, default is "bullet"

- `strength` *(number, optional)* — Strength scaling, default is 1.0

- `maxDist` *(number, optional)* — Maximum distance, default is 100.0

**Example:**

```lua
function tick()
	Shoot(Vec(0, 10, 0), Vec(0, -1, 0), "shotgun")
end
```

---

### [API] Paint

```lua
Paint(origin, radius, [type], [probability])
```

Tint the color of objects within radius to either black or yellow.

**Arguments:**

- `origin` *(TVec)* — Origin in world space as vector

- `radius` *(number)* — Affected radius, in range 0.0 to 5.0

- `type` *(string, optional)* — Paint type. Can be "explosion" or "spraycan". Default is spraycan.

- `probability` *(number, optional)* — Dithering probability between zero and one, default is 1.0

**Example:**

```lua
function tick()
	Paint(Vec(0, 2, 0), 5.0, "spraycan")
end
```

---

### [API] PaintRGBA

```lua
PaintRGBA(origin, radius, red, green, blue, [alpha], [probability])
```

Tint the color of objects within radius to custom RGBA color.

**Arguments:**

- `origin` *(TVec)* — Origin in world space as vector

- `radius` *(number)* — Affected radius, in range 0.0 to 5.0

- `red` *(number)* — red color value, in range 0.0 to 1.0

- `green` *(number)* — green color value, in range 0.0 to 1.0

- `blue` *(number)* — blue color value, in range 0.0 to 1.0

- `alpha` *(number, optional)* — alpha channel value, in range 0.0 to 1.0

- `probability` *(number, optional)* — Dithering probability between zero and one, default is 1.0

**Example:**

```lua
function tick()
	PaintRGBA(Vec(0, 5, 0), 5.5, 1.0, 0.0, 0.0)
end
```

---

### [API] MakeHole

```lua
count = MakeHole(position, r0, [r1], [r2], [silent])
```

Make a hole in the environment. Radius is given in meters. Soft materials: glass, foliage, dirt, wood, plaster and plastic. Medium materials: concrete, brick and weak metal. Hard materials: hard metal and hard masonry.

**Arguments:**

- `position` *(TVec)* — Hole center point

- `r0` *(number)* — Hole radius for soft materials

- `r1` *(number, optional)* — Hole radius for medium materials. May not be bigger than r0. Default zero.

- `r2` *(number, optional)* — Hole radius for hard materials. May not be bigger than r1. Default zero.

- `silent` *(boolean, optional)* — Make hole without playing any break sounds.

**Example:**

```lua
function init()
	MakeHole(Vec(0, 0, 0), 5.0, 1.0)
end
```

---

### [API] Explosion

```lua
Explosion(pos, size)
```

**Arguments:**

- `pos` *(TVec)* — Position in world space as vector

- `size` *(number)* — Explosion size from 0.5 to 4.0

**Example:**

```lua
function init()
	Explosion(Vec(0, 5, 0), 1)
end
```

---

### [API] SpawnFire

```lua
SpawnFire(pos)
```

**Arguments:**

- `pos` *(TVec)* — Position in world space as vector

**Example:**

```lua
function tick()
	SpawnFire(Vec(0, 2, 0))
end
```

---

### [API] GetFireCount

```lua
count = GetFireCount()
```

**Example:**

```lua
function tick()
	local c = GetFireCount()
	DebugPrint("Fire count " .. c)
end
```

---

### [API] QueryClosestFire

```lua
hit, pos = QueryClosestFire(origin, maxDist)
```

**Arguments:**

- `origin` *(TVec)* — World space position as vector

- `maxDist` *(number)* — Maximum search distance

**Example:**

```lua
function tick()
	local hit, pos = QueryClosestFire(GetPlayerTransform().pos, 5.0)
	if hit then
		--There is a fire within 5 meters to the player. Mark it with a debug cross.
		DebugCross(pos)
	end
end
```

---

### [API] QueryAabbFireCount

```lua
count = QueryAabbFireCount(min, max)
```

**Arguments:**

- `min` *(TVec)* — Aabb minimum point

- `max` *(TVec)* — Aabb maximum point

**Example:**

```lua
function tick()
	local count = QueryAabbFireCount(Vec(0,0,0), Vec(10,10,10))
	DebugPrint(count)
end
```

---

### [API] RemoveAabbFires

```lua
count = RemoveAabbFires(min, max)
```

**Arguments:**

- `min` *(TVec)* — Aabb minimum point

- `max` *(TVec)* — Aabb maximum point

**Example:**

```lua
function tick()
	local removedCount= RemoveAabbFires(Vec(0,0,0), Vec(10,10,10))
	DebugPrint(removedCount)
end
```

---

### [API] GetCameraTransform

```lua
transform = GetCameraTransform()
```

**Example:**

```lua
function tick()
	local t = GetCameraTransform()
	DebugPrint(TransformStr(t))
end
```

---

### [API] SetCameraTransform

```lua
SetCameraTransform(transform, [fov])
```

Override current camera transform for this frame. Call continuously to keep overriding. When transform of some shape or body used to calculate camera transform, consider use of AttachCameraTo, because you might be using transform from previous physics update (that was on previous frame or even earlier depending on fps and timescale).

**Arguments:**

- `transform` *(TTransform)* — Desired camera transform

- `fov` *(number, optional)* — Optional horizontal field of view in degrees (default: 90)

**Example:**

```lua
function tick()
	SetCameraTransform(Transform(Vec(0, 10, 0), QuatEuler(0, 90, 0)))
end
```

---

### [API] RequestFirstPerson

```lua
RequestFirstPerson(transition)
```

Use this function to switch to first-person view, overriding the player's selected third-person view. This is particularly useful for scenarios like looking through a camera viewfinder or a rifle scope. Call the function continuously to maintain the override.

**Arguments:**

- `transition` *(boolean)* — Use transition

**Example:**

```lua
function tick()
	if useViewFinder then
		RequestFirstPerson(true)
	end
end

function draw()
	if useViewFinder and !GetBool("game.thirdperson") then
		-- Draw view finder overlay
	end
end
```

---

### [API] RequestThirdPerson

```lua
RequestThirdPerson(transition)
```

Use this function to switch to third-person view, overriding the player's selected first-person view. Call the function continuously to maintain the override.

**Arguments:**

- `transition` *(boolean)* — Use transition

**Example:**

```lua
function tick()
	if useThirdPerson then
		RequestThirdPerson(true)
	end
end
```

---

### [API] SetCameraOffsetTransform

```lua
SetCameraOffsetTransform(transform, [stackable])
```

Call this function continously to apply a camera offset. Can be used for camera effects such as shake and wobble.

**Arguments:**

- `transform` *(TTransform)* — Desired camera offset transform

- `stackable` *(boolean, optional)* — True if camera offset should summ up with multiple calls per tick

**Example:**

```lua
function tick()
	local tPosX = Transform(Vec(math.sin(GetTime()*3.0) * 0.2, 0, 0))
	local tPosY = Transform(Vec(0, math.cos(GetTime()*3.0) * 0.2, 0), QuatAxisAngle(Vec(0, 0, 0)))

	SetCameraOffsetTransform(tPosX, true)
	SetCameraOffsetTransform(tPosY, true)
end
```

---

### [API] AttachCameraTo

```lua
AttachCameraTo(handle, [ignoreRotation])
```

Attach current camera transform for this frame to body or shape. Call continuously to keep overriding. In tick function we have coordinates of bodies and shapes that are not yet updated by physics, that's why camera can not be in sync with it using SetCameraTransform, instead use this function and SetCameraOffsetTransform to place camera around any body or shape without lag.

**Arguments:**

- `handle` *(number)* — Body or shape handle

- `ignoreRotation` *(boolean, optional)* — True to ignore rotation and use position only, false to use full transform

**Example:**

```lua
function tick()
	local vehicle = GetPlayerVehicle()
	if vehicle ~= 0 then
		AttachCameraTo(GetVehicleBody(vehicle))
		SetCameraOffsetTransform(Transform(Vec(1, 2, 3)))
	end
end
```

---

### [API] SetPivotClipBody

```lua
SetPivotClipBody(bodyHandle, mainShapeIdx)
```

treated as pivots when clipping body's shapes which is used to calculate clipping parameters (default: -1) Enforce camera clipping for this frame and mark the given body as a pivot for clipping. Call continuously to keep overriding.

**Arguments:**

- `bodyHandle` *(number)* — Handle of a body, shapes of which should be

- `mainShapeIdx` *(number)* — Optional index of a shape among the given

**Example:**

```lua
local body_1 = 0
local body_2 = 0
function init()
	body_1 = FindBody("body_1")
	body_2 = FindBody("body_2")
end

function tick()
	SetPivotClipBody(body_1, 0) -- this overload should be called once and
	-- only once per frame to take effect
	SetPivotClipBody(body_2)
end
```

---

### [API] ShakeCamera

```lua
ShakeCamera(strength)
```

Shakes the player camera

**Arguments:**

- `strength` *(number)* — Normalized strength of shaking

**Example:**

```lua
function tick()
	ShakeCamera(0.5)
end
```

---

### [API] SetCameraFov

```lua
SetCameraFov(degrees)
```

Override field of view for the next frame for all camera modes, except when explicitly set in SetCameraTransform

**Arguments:**

- `degrees` *(number)* — Horizontal field of view in degrees (10-170)

**Example:**

```lua
function tick()
	SetCameraFov(60)
end
```

---

### [API] SetCameraDof

```lua
SetCameraDof(distance, [amount])
```

Override depth of field for the next frame for all camera modes. Depth of field will be used even if turned off in options.

**Arguments:**

- `distance` *(number)* — Depth of field distance

- `amount` *(number, optional)* — Optional amount of blur (default 1.0)

**Example:**

```lua
function tick()
	--Set depth of field to 10 meters
	SetCameraDof(10)
end
```

---

### [API] PointLight

```lua
PointLight(pos, r, g, b, [intensity])
```

Add a temporary point light to the world for this frame. Call continuously for a steady light.

**Arguments:**

- `pos` *(TVec)* — World space light position

- `r` *(number)* — Red

- `g` *(number)* — Green

- `b` *(number)* — Blue

- `intensity` *(number, optional)* — Intensity. Default is 1.0.

**Example:**

```lua
function tick()
	--Pulsating, yellow light above world origo
	local intensity = 3 + math.sin(GetTime())
	PointLight(Vec(0, 5, 0), 1, 1, 0, intensity)
end
```

---

### [API] SetTimeScale

```lua
SetTimeScale(scale)
```

Experimental. Scale time in order to make a slow-motion or acceleration effect. Audio will also be affected. (v1.4 and below: this function will affect physics behavior and is not intended for gameplay purposes.) Starting from v1.5 this function does not affect physics behavior and rely on rendering interpolation. Scaling time up may decrease performance, and is not recommended for gameplay purposes. Calling this function will change time scale for the next frame only. Call every frame from tick function to get steady slow-motion.

**Arguments:**

- `scale` *(number)* — Time scale 0.0 to 2.0

**Example:**

```lua
function tick()
	--Slow down time when holding down a key
	if InputDown('t') then
		SetTimeScale(0.2)
	end
end
```

---

### [API] SetEnvironmentDefault

```lua
SetEnvironmentDefault()
```

Reset the environment properties to default. This is often useful before setting up a custom environment.

**Example:**

```lua
function init()
	SetEnvironmentDefault()
end
```

---

### [API] SetEnvironmentProperty

```lua
SetEnvironmentProperty(name, value0, [value1], [value2], [value3])
```

This function is used for manipulating the environment properties. The available properties are exactly the same as in the editor, except for "snowonground" which is not currently supported.

**Arguments:**

- `name` *(string)* — Property name

- `value0` *(any)* — Property value (type depends on property)

- `value1` *(any, optional)* — Extra property value (only some properties)

- `value2` *(any, optional)* — Extra property value (only some properties)

- `value3` *(any, optional)* — Extra property value (only some properties)

**Example:**

```lua
function init()
	SetEnvironmentDefault()
	SetEnvironmentProperty("skybox", "cloudy.dds")
	SetEnvironmentProperty("rain", 0.7)
	SetEnvironmentProperty("fogcolor", 0.5, 0.5, 0.8)
	SetEnvironmentProperty("nightlight", false)
end
```

---

### [API] GetEnvironmentProperty

```lua
value0, value1, value2, value3, value4 = GetEnvironmentProperty(name)
```

This function is used for querying the current environment properties. The available properties are exactly the same as in the editor.

**Arguments:**

- `name` *(string)* — Property name

**Example:**

```lua
function init()
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

---

### [API] SetPostProcessingDefault

```lua
SetPostProcessingDefault()
```

Reset the post processing properties to default.

**Example:**

```lua
function tick()
	SetPostProcessingProperty("saturation", 0.4)
	SetPostProcessingProperty("colorbalance", 1.3, 1.0, 0.7)
	SetPostProcessingDefault()
end
```

---

### [API] SetPostProcessingProperty

```lua
SetPostProcessingProperty(name, value0, [value1], [value2])
```

This function is used for manipulating the post processing properties. The available properties are exactly the same as in the editor.

**Arguments:**

- `name` *(string)* — Property name

- `value0` *(number)* — Property value

- `value1` *(number, optional)* — Extra property value (only some properties)

- `value2` *(number, optional)* — Extra property value (only some properties)

**Example:**

```lua
--Sepia post processing
function tick()
	SetPostProcessingProperty("saturation", 0.4)
	SetPostProcessingProperty("colorbalance", 1.3, 1.0, 0.7)
end
```

---

### [API] GetPostProcessingProperty

```lua
value0, value1, value2 = GetPostProcessingProperty(name)
```

This function is used for querying the current post processing properties. The available properties are exactly the same as in the editor.

**Arguments:**

- `name` *(string)* — Property name

**Example:**

```lua
function tick()
	SetPostProcessingProperty("saturation", 0.4)
	SetPostProcessingProperty("colorbalance", 1.3, 1.0, 0.7)
	local saturation = GetPostProcessingProperty("saturation")
	local r,g,b = GetPostProcessingProperty("colorbalance")
	DebugPrint("saturation " .. saturation)
	DebugPrint("colorbalance " .. r .. " " .. g .. " " .. b)
end
```

---

### [API] DrawLine

```lua
DrawLine(p0, p1, [r], [g], [b], [a])
```

Draw a 3D line. In contrast to DebugLine, it will not show behind objects. Default color is white.

**Arguments:**

- `p0` *(TVec)* — World space point as vector

- `p1` *(TVec)* — World space point as vector

- `r` *(number, optional)* — Red

- `g` *(number, optional)* — Green

- `b` *(number, optional)* — Blue

- `a` *(number, optional)* — Alpha

**Example:**

```lua
function tick()
	--Draw white debug line
	DrawLine(Vec(0, 0, 0), Vec(-10, 5, -10))

	--Draw red debug line
	DrawLine(Vec(0, 0, 0), Vec(10, 5, 10), 1, 0, 0)
end
```

---

### [API] DebugLine

```lua
DebugLine(p0, p1, [r], [g], [b], [a])
```

Draw a 3D debug overlay line in the world. Default color is white.

**Arguments:**

- `p0` *(TVec)* — World space point as vector

- `p1` *(TVec)* — World space point as vector

- `r` *(number, optional)* — Red

- `g` *(number, optional)* — Green

- `b` *(number, optional)* — Blue

- `a` *(number, optional)* — Alpha

**Example:**

```lua
function tick()
	--Draw white debug line
	DebugLine(Vec(0, 0, 0), Vec(-10, 5, -10))

	--Draw red debug line
	DebugLine(Vec(0, 0, 0), Vec(10, 5, 10), 1, 0, 0)
end
```

---

### [API] DebugCross

```lua
DebugCross(p0, [r], [g], [b], [a])
```

Draw a debug cross in the world to highlight a location. Default color is white.

**Arguments:**

- `p0` *(TVec)* — World space point as vector

- `r` *(number, optional)* — Red

- `g` *(number, optional)* — Green

- `b` *(number, optional)* — Blue

- `a` *(number, optional)* — Alpha

**Example:**

```lua
function tick()
	DebugCross(Vec(10, 5, 5))
end
```

---

### [API] DebugTransform

```lua
DebugTransform(transform, [scale])
```

Draw the axis of the transform

**Arguments:**

- `transform` *(TTransform)* — The transform

- `scale` *(number, optional)* — Length of the axis

**Example:**

```lua
function tick()
	DebugTransform(GetPlayerCameraTransform(), 0.5)
end
```

---

### [API] DebugWatch

```lua
DebugWatch(name, value, [lineWrapping])
```

Show a named valued on screen for debug purposes. Up to 32 values can be shown simultaneously. Values updated the current frame are drawn opaque. Old values are drawn transparent in white. The function will also recognize tables and convert them to strings automatically.

**Arguments:**

- `name` *(string)* — Name

- `value` *(string)* — Value

- `lineWrapping` *(boolean, optional)* — True if you need to wrap Table lines. Works only with tables.

**Example:**

```lua
function tick()
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

---

### [API] DebugPrint

```lua
DebugPrint(message, [lineWrapping])
```

Display message on screen. The last 20 lines are displayed. The function will also recognize tables and convert them to strings automatically.

**Arguments:**

- `message` *(string)* — Message to display

- `lineWrapping` *(boolean, optional)* — True if you need to wrap Table lines. Works only with tables.

**Example:**

```lua
function init()
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

---

### [API] RegisterListenerTo

```lua
RegisterListenerTo(eventName, listenerFunction)
```

Binds the callback function on the event

**Arguments:**

- `eventName` *(string)* — Event name

- `listenerFunction` *(string)* — Listener function name

**Example:**

```lua
function onLangauageChanged()
	DebugPrint("langauageChanged")
end

function init()
	RegisterListenerTo("LanguageChanged", "onLangauageChanged")
	TriggerEvent("LanguageChanged")
end
```

---

### [API] UnregisterListener

```lua
UnregisterListener(eventName, listenerFunction)
```

Unbinds the callback function from the event

**Arguments:**

- `eventName` *(string)* — Event name

- `listenerFunction` *(string)* — Listener function name

**Example:**

```lua
function onLangauageChanged()
	DebugPrint("langauageChanged")
end

function init()
	RegisterListenerTo("LanguageChanged", "onLangauageChanged")
	UnregisterListener("LanguageChanged", "onLangauageChanged")
	TriggerEvent("LanguageChanged")
end
```

---

### [API] TriggerEvent

```lua
TriggerEvent(eventName, [args])
```

Triggers an event for all registered listeners

**Arguments:**

- `eventName` *(string)* — Event name

- `args` *(string, optional)* — Event parameters

**Example:**

```lua
function onLangauageChanged()
	DebugPrint("langauageChanged")
end

function init()
	RegisterListenerTo("LanguageChanged", "onLangauageChanged")
	UnregisterListener("LanguageChanged", "onLangauageChanged")
	TriggerEvent("LanguageChanged")
end
```

---

### [API] LoadHaptic

```lua
handle = LoadHaptic(filepath)
```

**Arguments:**

- `filepath` *(string)* — Path to Haptic effect to play

**Example:**

```lua
-- Rumble with gun Haptic effect
function init()
	haptic_effect = LoadHaptic("haptic/gun_fire.xml")
end

function tick()
	if trigHaptic then
		PlayHaptic(haptic_effect, 1)
	end
end
```

---

### [API] CreateHaptic

```lua
handle = CreateHaptic(leftMotorRumble, rightMotorRumble, leftTriggerRumble, rightTriggerRumble)
```

**Arguments:**

- `leftMotorRumble` *(number)* — Amount of rumble for left motor

- `rightMotorRumble` *(number)* — Amount of rumble for right motor

- `leftTriggerRumble` *(number)* — Amount of rumble for left trigger

- `rightTriggerRumble` *(number)* — Amount of rumble for right trigger

**Example:**

```lua
-- Rumble with gun Haptic effect
function init()
	haptic_effect = CreateHaptic(1, 1, 0, 0)
end

function tick()
	if trigHaptic then
		PlayHaptic(haptic_effect, 1)
	end
end
```

---

### [API] PlayHaptic

```lua
PlayHaptic(handle, amplitude)
```

If Haptic already playing, restarts it.

**Arguments:**

- `handle` *(string)* — Handle of haptic effect

- `amplitude` *(number)* — Amplidute used for calculation of Haptic effect.

**Example:**

```lua
-- Rumble with gun Haptic effect
function init()
	haptic_effect = LoadHaptic("haptic/gun_fire.xml")
end

function tick()
	if trigHaptic then
		PlayHaptic(haptic_effect, 1)
	end
end
```

---

### [API] PlayHapticDirectional

```lua
PlayHapticDirectional(handle, direction, amplitude)
```

If Haptic already playing, restarts it.

**Arguments:**

- `handle` *(string)* — Handle of haptic effect

- `direction` *(TVec)* — Direction in which effect must be played

- `amplitude` *(number)* — Amplidute used for calculation of Haptic effect.

**Example:**

```lua
-- Rumble with gun Haptic effect
local haptic_effect
function init()
	haptic_effect = LoadHaptic("haptic/gun_fire.xml")
end

function tick()
	if InputPressed("interact") then
		PlayHapticDirectional(haptic_effect, Vec(-1, 0, 0), 1)
	end
end
```

---

### [API] HapticIsPlaying

```lua
flag = HapticIsPlaying(handle)
```

**Arguments:**

- `handle` *(string)* — Handle of haptic effect

**Example:**

```lua
-- Rumble infinitely
local haptic_effect
function init()
	haptic_effect = LoadHaptic("haptic/gun_fire.xml")
end

function tick()
	if not HapticIsPlaying(haptic_effect) then
		PlayHaptic(haptic_effect, 1)
	end
end
```

---

### [API] SetToolHaptic

```lua
SetToolHaptic(id, handle, [amplitude])
```

Register haptic as a "Tool haptic" for custom tools. "Tool haptic" will be played on repeat while this tool is active. Also it can be used for Active Triggers of DualSense controller

**Arguments:**

- `id` *(string)* — Tool unique identifier

- `handle` *(string)* — Handle of haptic effect

- `amplitude` *(number, optional)* — Amplitude multiplier. Default (1.0)

**Example:**

```lua
function init()
	RegisterTool("minigun", "loc@MINIGUN", "MOD/vox/minigun.vox")
	toolHaptic = LoadHaptic("MOD/haptic/tool.xml")
	SetToolHaptic("minigun", toolHaptic) 
end
```

---

### [API] StopHaptic

```lua
StopHaptic(handle)
```

**Arguments:**

- `handle` *(string)* — Handle of haptic effect

**Example:**

```lua
-- Rumble infinitely
local haptic_effect
function init()
	haptic_effect = LoadHaptic("haptic/gun_fire.xml")
end

function tick()
    if InputDown("interact") then
        StopHaptic(haptic_effect)
    elseif not HapticIsPlaying(haptic_effect) then
		PlayHaptic(haptic_effect, 1)
    end
end
```

---

### [API] SetVehicleHealth

```lua
SetVehicleHealth(vehicle, health)
```

Works only for vehicles with 'customhealth' tag. 'customhealth' disables the common vehicles damage system. So this function needed for custom vehicle damage systems.

**Arguments:**

- `vehicle` *(number)* — Vehicle handle

- `health` *(number)* — Set vehicle health (between zero and one)

**Example:**

```lua
function tick()
	if InputPressed("usetool") then
		SetVehicleHealth(FindVehicle("car", true), 0.0)
	end
end
```

---

### [API] QueryRaycastWater

```lua
hit, dist, hitPos = QueryRaycastWater(origin, direction, maxDist)
```

This will perform a raycast query looking for water.

**Arguments:**

- `origin` *(TVec)* — Raycast origin as world space vector

- `direction` *(TVec)* — Unit length raycast direction as world space vector

- `maxDist` *(number)* — Raycast maximum distance. Keep this as low as possible for good performance.

**Example:**

```lua
function init()
	--Raycast from a high point straight downwards, looking for water
	local hit, d = QueryRaycast(Vec(0, 100, 0), Vec(0, -1, 0), 100)
	if hit then
		DebugPrint(d)
	end
end
```

---

### [API] AddHeat

```lua
AddHeat(shape, pos, amount)
```

Adds heat to shape. It works similar to blowtorch. As soon as the heat of the voxel reaches a critical value, it destroys and can ignite the surrounding voxels.

**Arguments:**

- `shape` *(number)* — Shape handle

- `pos` *(TVec)* — World space point as vector

- `amount` *(number)* — amount of heat

**Example:**

```lua
function tick(dt)
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

---

### [API] GetGravity

```lua
vector = GetGravity()
```

Returns the gravity value on the scene.

**Example:**

```lua
function tick()
	DebugPrint(VecStr(GetGravity()))
end
```

---

### [API] SetGravity

```lua
SetGravity(vec)
```

Sets the gravity value on the scene. When the scene restarts, it resets to the default value (0, -10, 0).

**Arguments:**

- `vec` *(TVec)* — Gravity vector

**Example:**

```lua
local isMoonGravityEnabled = false

function tick()
	if InputPressed("g") then
		isMoonGravityEnabled = not isMoonGravityEnabled
		if isMoonGravityEnabled then
			SetGravity(Vec(0, -1.6, 0))
		else
			SetGravity(Vec(0, -10.0, 0))
		end
	end
end
```

---

### [API] GetFps

```lua
fps = GetFps()
```

Returns the fps value based on general game timestep. It doesn't depend on whether it is called from tick or update.

**Example:**

```lua
function tick()
	DebugWatch("fps", GetFps())
end
```

---


---
**Navigation:**
[Teardown scripting](00_overview.md) | [Parameters](01_parameters.md) | [Script control](02_script_control.md) | [Registry](03_registry.md) | [Vector math](04_vector_math.md) | [Entity](05_entity.md) | [Body](06_body.md) | [Shape](07_shape.md) | [Location](08_location.md) | [Joint](09_joint.md) | [Animation](10_animation.md) | [Light](11_light.md) | [Trigger](12_trigger.md) | [Screen](13_screen.md) | [Vehicle](14_vehicle.md) | [Player](15_player.md) | [Sound](16_sound.md) | [Sprite](17_sprite.md) | [Scene queries](18_scene_queries.md) | [Particles](19_particles.md) | [Spawning](20_spawning.md) | **Miscellaneous** | [User Interface](22_ui.md)