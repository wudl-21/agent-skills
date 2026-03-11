# Teardown SP API — Player
> **Version:** 1.7.0 | **Mode:** Singleplayer | **Language:** Lua 5.1

The player functions expose certain information about the player.

---

### [API] GetPlayerPos *(deprecated)*

```lua
position = GetPlayerPos()
```

Return center point of player. This function is deprecated. Use GetPlayerTransform instead.

**Example:**

```lua
function init()
	local p = GetPlayerPos()
	DebugPrint(p)

	--This is equivalent to
	p = VecAdd(GetPlayerTransform().pos, Vec(0,1,0))
	DebugPrint(p)
end
```

---

### [API] GetPlayerAimInfo

```lua
hit, startpos, endpos, direction, hitnormal, hitdist, hitentity, hitmaterial = GetPlayerAimInfo(position, [maxdist])
```

**Arguments:**

- `position` *(TVec)* — Start position of the search

- `maxdist` *(number, optional)* — Max search distance

**Example:**

```lua
local muzzle = GetToolLocationWorldTransform("muzzle")
local _, pos, _, dir = GetPlayerAimInfo(muzzle.pos)
Shoot(pos, dir)
```

---

### [API] GetPlayerPitch

```lua
pitch = GetPlayerPitch()
```

The player pitch angle is applied to the player camera transform. This value can be used to animate tool pitch movement when using SetToolTransformOverride.

**Example:**

```lua
function init()
	local pitchRotation = Quat(Vec(1,0,0), GetPlayerPitch())
end
```

---

### [API] GetPlayerYaw

```lua
yaw = GetPlayerYaw()
```

The player yaw angle is applied to the player camera transform. It represents the top-down angle of rotation of the player.

**Example:**

```lua
function init()
	local compassBearing = GetPlayerYaw()
end
```

---

### [API] SetPlayerPitch

```lua
SetPlayerPitch(pitch)
```

Sets the player pitch.

**Arguments:**

- `pitch` *(number)* — Pitch.

**Example:**

```lua
function tick()
	-- look straight ahead
	SetPlayerPitch(0.0)
end
```

---

### [API] GetPlayerCrouch

```lua
recoil = GetPlayerCrouch()
```

**Example:**

```lua
function tick()
    local crouch = GetPlayerCrouch()
    if crouch > 0.0 then
        ...
    end
end
```

---

### [API] GetPlayerTransform

```lua
transform = GetPlayerTransform([includePitch])
```

The player transform is located at the bottom of the player. The player transform considers heading (looking left and right). Forward is along negative Z axis. Player pitch (looking up and down) does not affect player transform unless includePitch is set to true. If you want the transform of the eye, use GetPlayerCameraTransform() instead.

**Arguments:**

- `includePitch` *(boolean, optional)* — Include the player pitch (look up/down) in transform

**Example:**

```lua
function init()
	local t = GetPlayerTransform()
	DebugPrint(TransformStr(t))
end
```

---

### [API] SetPlayerTransform

```lua
SetPlayerTransform(transform, [includePitch])
```

Instantly teleport the player to desired transform. Unless includePitch is set to true, up/down look angle will be set to zero during this process. Player velocity will be reset to zero.

**Arguments:**

- `transform` *(TTransform)* — Desired player transform

- `includePitch` *(boolean, optional)* — Set player pitch (look up/down) as well

**Example:**

```lua
function tick()
	if InputPressed("jump") then
		local t = Transform(Vec(50, 0, 0), QuatEuler(0, 90, 0))
		SetPlayerTransform(t)
	end
end
```

---

### [API] ClearPlayerRig

```lua
ClearPlayerRig(rig-id)
```

**Arguments:**

- `rig-id` *(number)* — Unique rig-id, -1 means all rigs

**Example:**

```lua
    --Clear specific rig
    ClearPlayerRig(someId)
    
    --Clear all rigs
    ClearPlayerRig(-1)
```

---

### [API] SetPlayerRigLocationLocalTransform

```lua
SetPlayerRigLocationLocalTransform(rig-id, name, location)
```

**Arguments:**

- `rig-id` *(number)* — Unique id

- `name` *(string)* — Name of location

- `location` *(table)* — Rig Local transform of the location

**Example:**

```lua
    local someBody = FindBody("bodyname")
    SetPlayerRigLocationLocalTransform(someBody, "ik_foot_l", TransformToLocalTransform(GetBodyTransform(someBody), GetLocationTransform(FindLocation("ik_foot_l"))))
```

---

### [API] SetPlayerRigTransform

```lua
SetPlayerRigTransform(rig-id, location)
```

This will both update the rig identified by the 'id' and make it active

**Arguments:**

- `rig-id` *(number)* — Unique id

- `location` *(table)* — New world transform

**Example:**

```lua
    local someBody = FindBody("bodyname")
    SetPlayerRigTransform(someBody, GetBodyTransform(someBody))
```

---

### [API] GetPlayerRigTransform

```lua
location = GetPlayerRigTransform()
```

---

### [API] GetPlayerRigLocationWorldTransform

```lua
location = GetPlayerRigLocationWorldTransform(name)
```

**Arguments:**

- `name` *(string)* — Name of location

**Example:**

```lua
local t = GetPlayerRigLocationWorldTransform("ik_hand_l")
```

---

### [API] SetPlayerRigTags

```lua
SetPlayerRigTags(tag, [value])
```

**Arguments:**

- `tag` *(string)* — Tag name

- `value` *(string, optional)* — Tag value

---

### [API] GetPlayerRigHasTag

```lua
exists = GetPlayerRigHasTag(tag)
```

**Arguments:**

- `tag` *(string)* — Tag name

---

### [API] GetPlayerRigTagValue

```lua
value = GetPlayerRigTagValue(tag)
```

**Arguments:**

- `tag` *(string)* — Tag name

---

### [API] SetPlayerGroundVelocity

```lua
SetPlayerGroundVelocity(vel)
```

Make the ground act as a conveyor belt, pushing the player even if ground shape is static.

**Arguments:**

- `vel` *(TVec)* — Desired ground velocity

**Example:**

```lua
function tick()
	SetPlayerGroundVelocity(Vec(2,0,0))
end
```

---

### [API] GetPlayerEyeTransform

```lua
transform = GetPlayerEyeTransform()
```

The player eye transform is the same as what you get from GetCameraTransform when playing in first-person, but if you have set a camera transform manually with SetCameraTransform or playing in third-person, you can retrieve the player eye transform with this function.

**Example:**

```lua
function init()
	local t = GetPlayerEyeTransform()
	DebugPrint(TransformStr(t))
end
```

---

### [API] GetPlayerCameraTransform

```lua
transform = GetPlayerCameraTransform()
```

The player camera transform is usually the same as what you get from GetCameraTransform, but if you have set a camera transform manually with SetCameraTransform, you can retrieve the standard player camera transform with this function.

**Example:**

```lua
function init()
	local t = GetPlayerCameraTransform()
	DebugPrint(TransformStr(t))
end
```

---

### [API] SetPlayerCameraOffsetTransform

```lua
SetPlayerCameraOffsetTransform(transform, [stackable])
```

Call this function continously to apply a camera offset. Can be used for camera effects such as shake and wobble.

**Arguments:**

- `transform` *(TTransform)* — Desired player camera offset transform

- `stackable` *(boolean, optional)* — True if eye offset should summ up with multiple calls per tick

**Example:**

```lua
function tick()
	local t = Transform(Vec(), QuatAxisAngle(Vec(1, 0, 0), math.sin(GetTime()*3.0) * 3.0))
	SetPlayerCameraOffsetTransform(t)
end
```

---

### [API] SetPlayerSpawnTransform

```lua
SetPlayerSpawnTransform(transform)
```

Call this function during init to alter the player spawn transform.

**Arguments:**

- `transform` *(TTransform)* — Desired player spawn transform

**Example:**

```lua
function init()
	local t = Transform(Vec(10, 0, 0), QuatEuler(0, 90, 0))
	SetPlayerSpawnTransform(t)
end
```

---

### [API] SetPlayerSpawnHealth

```lua
SetPlayerSpawnHealth(health)
```

Call this function during init to alter the player spawn health amount.

**Arguments:**

- `health` *(number)* — Desired player spawn health (between zero and one)

**Example:**

```lua
function init()
	SetPlayerSpawnHealth(0.5)
end
```

---

### [API] SetPlayerSpawnTool

```lua
SetPlayerSpawnTool(id)
```

Call this function during init to alter the player spawn active tool.

**Arguments:**

- `id` *(string)* — Tool unique identifier

**Example:**

```lua
function init()
	SetPlayerSpawnTool("pistol")
end
```

---

### [API] GetPlayerVelocity

```lua
velocity = GetPlayerVelocity()
```

**Example:**

```lua
function tick()
	local vel = GetPlayerVelocity()
	DebugPrint(VecStr(vel))
end
```

---

### [API] SetPlayerVehicle

```lua
SetPlayerVehicle(vehicle)
```

Drive specified vehicle.

**Arguments:**

- `vehicle` *(number)* — Handle to vehicle or zero to not drive.

**Example:**

```lua
function tick()
	if InputPressed("interact") then
		local car = FindVehicle("mycar")
		SetPlayerVehicle(car)
	end
end
```

---

### [API] SetPlayerAnimator

```lua
SetPlayerAnimator(animator)
```

**Arguments:**

- `animator` *(number)* — Handle to animator or zero for no animator

---

### [API] GetPlayerAnimator

```lua
animator = GetPlayerAnimator()
```

---

### [API] SetPlayerVelocity

```lua
SetPlayerVelocity(velocity)
```

**Arguments:**

- `velocity` *(TVec)* — Player velocity in world space as vector

**Example:**

```lua
function tick()
	if InputPressed("jump") then
		SetPlayerVelocity(Vec(0, 5, 0)) 
	end
end
```

---

### [API] GetPlayerVehicle

```lua
handle = GetPlayerVehicle()
```

**Example:**

```lua
function tick()
	local vehicle = GetPlayerVehicle()
	if vehicle ~= 0 then
		DebugPrint("Player drives the vehicle")
	end
end
```

---

### [API] IsPlayerGrounded

```lua
isGrounded = IsPlayerGrounded()
```

**Example:**

```lua
local isGrounded = IsPlayerGrounded()
```

---

### [API] GetPlayerGroundContact

```lua
contact, shape, point, normal = GetPlayerGroundContact()
```

Get information about player ground contact. If the output boolean (contact) is false then the rest of the output is invalid.

**Example:**

```lua
function tick()
	hasGroundContact, shape, point, normal = GetPlayerGroundContact()

	if hasGroundContact then
		-- print ground contact data
		DebugPrint(VecStr(point).." : "..VecStr(normal))
	end
end
```

---

### [API] GetPlayerGrabShape

```lua
handle = GetPlayerGrabShape()
```

**Example:**

```lua
function tick()
	local shape = GetPlayerGrabShape()
	if shape ~= 0 then
		DebugPrint("Player is grabbing a shape")
	end
end
```

---

### [API] GetPlayerGrabBody

```lua
handle = GetPlayerGrabBody()
```

**Example:**

```lua
function tick()
	local body = GetPlayerGrabBody()
	if body ~= 0 then
		DebugPrint("Player is grabbing a body")
	end
end
```

---

### [API] ReleasePlayerGrab

```lua
ReleasePlayerGrab()
```

Release what the player is currently holding

**Example:**

```lua
function tick()
	if InputPressed("jump") then
		ReleasePlayerGrab()
	end
end
```

---

### [API] GetPlayerGrabPoint

```lua
pos = GetPlayerGrabPoint()
```

**Example:**

```lua
local body = GetPlayerGrabBody()
if body ~= 0 then
	local pos = GetPlayerGrabPoint()
end
```

---

### [API] GetPlayerPickShape

```lua
handle = GetPlayerPickShape()
```

**Example:**

```lua
function tick()
	local shape = GetPlayerPickShape()
	if shape ~= 0 then
		DebugPrint("Picked shape " .. shape)
	end
end
```

---

### [API] GetPlayerPickBody

```lua
handle = GetPlayerPickBody()
```

**Example:**

```lua
function tick()
	local body = GetPlayerPickBody()
	if body ~= 0 then
		DebugWatch("Pick body ", body)
	end
end
```

---

### [API] GetPlayerInteractShape

```lua
handle = GetPlayerInteractShape()
```

Interactable shapes has to be tagged with "interact". The engine determines which interactable shape is currently interactable.

**Example:**

```lua
function tick()
	local shape = GetPlayerInteractShape()
	if shape ~= 0 then
		DebugPrint("Interact shape " .. shape)
	end
end
```

---

### [API] GetPlayerInteractBody

```lua
handle = GetPlayerInteractBody()
```

Interactable shapes has to be tagged with "interact". The engine determines which interactable body is currently interactable.

**Example:**

```lua
function tick()
	local body = GetPlayerInteractBody()
	if body ~= 0 then
		DebugPrint("Interact body " .. body)
	end
end
```

---

### [API] SetPlayerScreen

```lua
SetPlayerScreen(handle)
```

Set the screen the player should interact with. For the screen to feature a mouse pointer and receieve input, the screen also needs to have interactive property.

**Arguments:**

- `handle` *(number)* — Handle to screen or zero for no screen

**Example:**

```lua
function tick()
	if InputPressed("interact") then
		if GetPlayerScreen() ~= 0 then
			SetPlayerScreen(0)
		else
			SetPlayerScreen(screen)
		end

	end
end
```

---

### [API] GetPlayerScreen

```lua
handle = GetPlayerScreen()
```

**Example:**

```lua
function tick()
	if InputPressed("interact") then
		if GetPlayerScreen() ~= 0 then
			SetPlayerScreen(0)
		else
			SetPlayerScreen(screen)
		end

	end
end
```

---

### [API] SetPlayerHealth

```lua
SetPlayerHealth(health)
```

**Arguments:**

- `health` *(number)* — Set player health (between zero and one)

**Example:**

```lua
function tick()
	if InputPressed("interact") then
		if GetPlayerHealth() < 0.75 then
			SetPlayerHealth(1.0)
		else
			SetPlayerHealth(0.5)
		end
	end
end
```

---

### [API] GetPlayerHealth

```lua
health = GetPlayerHealth()
```

**Example:**

```lua
function tick()
	if InputPressed("interact") then
		if GetPlayerHealth() < 0.75 then
			SetPlayerHealth(1.0)
		else
			SetPlayerHealth(0.5)
		end
	end
end
```

---

### [API] SetPlayerRegenerationState

```lua
SetPlayerRegenerationState(state)
```

Enable or disable regeneration for player

**Arguments:**

- `state` *(boolean)* — State of player regeneration

**Example:**

```lua
function init()
	-- disable regeneration for player
	SetPlayerRegenerationState(false)
end
```

---

### [API] RespawnPlayer

```lua
RespawnPlayer()
```

Respawn player at spawn position without modifying the scene

**Example:**

```lua
function tick()
	if InputPressed("interact") then
		RespawnPlayer()
	end
end
```

---

### [API] GetPlayerWalkingSpeed

```lua
speed = GetPlayerWalkingSpeed()
```

This function gets base speed, but real player speed depends on many factors such as health, crouch, water, grabbing objects.

**Example:**

```lua
function tick()
	DebugPrint(GetPlayerWalkingSpeed())
end
```

---

### [API] SetPlayerWalkingSpeed

```lua
SetPlayerWalkingSpeed(speed)
```

This function sets base speed, but real player speed depends on many factors such as health, crouch, water, grabbing objects.

**Arguments:**

- `speed` *(number)* — Set player base walking speed

**Example:**

```lua
function tick()
	if InputDown("shift") then
		SetPlayerWalkingSpeed(15.0)
	else
		SetPlayerWalkingSpeed(7.0)
	end
end
```

---

### [API] GetPlayerParam

```lua
value = GetPlayerParam(parameter)
```

Param name Type Description health float Current value of the player's health.healthRegeneration boolean Is the player's health regeneration enabled.walkingSpeed float The player's walking speed.jumpSpeed float The player's jump speed.godMode boolean If the value is True, the player does not lose healthfriction float Player body frictionfrictionMode string Player friction combine modeflyMode boolean If the value is True, the player will fly

**Arguments:**

- `parameter` *(string)* — Parameter name

**Example:**

```lua
function tick()
	-- The parameter names are case-insensitive, so any of the specified writing styles will be correct:
	-- "GodMode", "godmode", "godMode"
	local paramName = "GodMode"
	local param = GetPlayerParam(paramName)
	DebugWatch(paramName, param)

	if InputPressed("g") then
		SetPlayerParam(paramName, not param)
	end
end
```

---

### [API] SetPlayerParam

```lua
SetPlayerParam(parameter, value)
```

Param name Type Description health float Current value of the player's health.healthRegeneration boolean Is the player's health regeneration enabled.walkingSpeed float The player's walking speed. This value is applied for 1 frame! jumpSpeed float The player's jump speed. The height of the jump depends non-linearly on the jump speed. This value is applied for 1 frame! godMode boolean If the value is True, the player does not lose healthfriction float Player body friction. Default is 0.8frictionMode string Player friction combine mode. Can be (average|minimum|multiply|maximum)flyMode boolean If the value is True, the player will fly

**Arguments:**

- `parameter` *(string)* — Parameter name

- `value` *(any)* — Parameter value

**Example:**

```lua
function tick()
	-- The parameter names are case-insensitive, so any of the specified writing styles will be correct:
	-- "JumpSpeed", "jumpspeed", "jumpSpeed"
	local paramName = "JumpSpeed"
	local param = GetPlayerParam(paramName)
	DebugWatch(paramName, param)

	if InputDown("shift") then
		-- JumpSpeed sets for 1 frame
		SetPlayerParam(paramName, 10)
	end
end
```

---

### [API] SetPlayerHidden

```lua
SetPlayerHidden()
```

Use this function to hide the player character.

**Example:**

```lua

function tick()
	...
	SetCameraTransform(t)
	SetPlayerHidden()
end
```

---

### [API] RegisterTool

```lua
RegisterTool(id, name, file, [group])
```

Register a custom tool that will show up in the player inventory and can be selected with scroll wheel. Do this only once per tool. You also need to enable the tool in the registry before it can be used.

**Arguments:**

- `id` *(string)* — Tool unique identifier

- `name` *(string)* — Tool name to show in hud

- `file` *(string)* — Path to vox file or prefab xml

- `group` *(number, optional)* — Tool group for this tool (1-6) Default is 6.

**Example:**

```lua
function init()
	RegisterTool("lasergun", "Laser Gun", "MOD/vox/lasergun.vox")
	SetBool("game.tool.lasergun.enabled", true)
end

function tick()
	if GetString("game.player.tool") == "lasergun" then
		--Tool is selected. Tool logic goes here.
	end
end
```

---

### [API] GetToolBody

```lua
handle = GetToolBody()
```

Return body handle of the visible tool. You can use this to retrieve tool shapes and animate them, change emissiveness, etc. Do not attempt to set the tool body transform, since it is controlled by the engine. Use SetToolTranform for that.

**Example:**

```lua
function tick()
	local toolBody = GetToolBody()
	if toolBody~=0 then
		DebugPrint("Tool body: " .. toolBody)
	end
end
```

---

### [API] GetToolHandPoseLocalTransform

```lua
right, left = GetToolHandPoseLocalTransform()
```

**Example:**

```lua
local right, left = GetToolHandPoseLocalTransform()
```

---

### [API] GetToolHandPoseWorldTransform

```lua
right, left = GetToolHandPoseWorldTransform()
```

**Example:**

```lua
local right, left = GetToolHandPoseWorldTransform()
```

---

### [API] SetToolHandPoseLocalTransform

```lua
SetToolHandPoseLocalTransform(right, left)
```

Use this function to position the character's hands on the currently equipped tool. This function must be called every frame from the tick function. In third-person view, failing to call this function can lead to different outcomes depending on how the tool is animated: If the tool's transform is not explicitly set or is set using SetToolTransform, not calling this function will trigger a fallback solution where the right hand is automatically positioned. If the tool is animated using the SetToolTransformOverride function, not calling this function will result in the character's animation taking control of the hand movement

**Arguments:**

- `right` *(TTransform)* — Transform of right hand relative to the tool body origin, or nil if right hand is not used

- `left` *(TTransform)* — Transform of left hand, or nil if left hand is not used

**Example:**

```lua
if GetBool("game.thirdperson") then
	if aiming then
		SetToolHandPoseLocalTransform(Transform(Vec(0.2,0.0,0.0), QuatAxisAngle(Vec(0,1,0), 90.0)), Transform(Vec(-0.1, 0.0, -0.4)))
	else
		SetToolHandPoseLocalTransform(Transform(Vec(0.2,0.0,0.0), QuatAxisAngle(Vec(0,1,0), 90.0)), nil)
	end
end
```

---

### [API] GetToolLocationLocalTransform

```lua
location = GetToolLocationLocalTransform(name)
```

Return transform of a tool location in tool space. Locations can be defined using the tool prefab editor.

**Arguments:**

- `name` *(string)* — Name of location

**Example:**

```lua
local right  = GetToolLocationLocalTransform("righthand")
SetToolHandPoseLocalTransform(right, nil)
```

---

### [API] GetToolLocationWorldTransform

```lua
location = GetToolLocationWorldTransform(name)
```

Return transform of a tool location in world space. Locations can be defined using the tool prefab editor. A tool location is defined in tool space and to get the world space transform a tool body is required. If a tool body does not exist this function will return nil.

**Arguments:**

- `name` *(string)* — Name of location

**Example:**

```lua
local muzzle = GetToolLocationWorldTransform("muzzle")
Shoot(muzzle, direction)
```

---

### [API] SetToolTransform

```lua
SetToolTransform(transform, [sway])
```

Apply an additional transform on the visible tool body. This can be used to create tool animations. You need to set this every frame from the tick function. The optional sway parameter control the amount of tool swaying when walking. Set to zero to disable completely.

**Arguments:**

- `transform` *(TTransform)* — Tool body transform

- `sway` *(number, optional)* — Tool sway amount. Default is 1.0.

**Example:**

```lua
function init()
	--Offset the tool half a meter to the right
	local offset = Transform(Vec(0.5, 0, 0))
	SetToolTransform(offset)
end
```

---

### [API] SetToolAllowedZoom

```lua
SetToolAllowedZoom(zoom, [zoom])
```

Set the allowed zoom for a registered tool. The zoom sensitivity will be factored with the user options for sensitivity.

**Arguments:**

- `zoom` *(number)* — Zoom factor

- `zoom` *(sensitivity], optional)* — Input sensitivity when zoomed in. Default is 1.0.

**Example:**

```lua
function tick()
	-- allow our scoped tool to zoom by factor 4.
	SetToolAllowedZoom(4.0, 0.5)
end
```

---

### [API] SetToolTransformOverride

```lua
SetToolTransformOverride(transform)
```

This function serves as an alternative to SetToolTransform, providing full control over tool animation by disabling all internal tool animations. When using this function, you must manually include pitch, sway, and crouch movements in the transform. To maintain this control, call the function every frame from the tick function.

**Arguments:**

- `transform` *(TTransform)* — Tool body transform

**Example:**

```lua
function init()

	if GetBool("game.thirdperson") then
		local toolTransform = Transform(Vec(0.3, -0.3, -0.2), Quat(0.0, 0.0, 15.0))

		-- Rotate around point
		local pivotPoint = Vec(-0.01, -0.2, 0.04)
		toolTransform.pos = VecSub(toolTransform.pos, pivotPoint)
		local rotation = Transform(Vec(), QuatAxisAngle(Vec(0,0,1), GetPlayerPitch()))
		toolTransform = TransformToParentTransform(rotation, toolTransform)
		toolTransform.pos = VecAdd(toolTransform.pos, pivotPoint)

		SetToolTransformOverride(toolTransform)
	else
		local toolTransform = Transform(Vec(0.3, -0.3, -0.2), Quat(0.0, 0.0, 15.0))
		SetToolTransform(toolTransform)
	end
end
```

---

### [API] SetToolOffset

```lua
SetToolOffset(offset)
```

Apply an additional offset on the visible tool body. This can be used to tweak tool placement for different characters. You need to set this every frame from the tick function.

**Arguments:**

- `offset` *(TVec)* — Tool body offset

**Example:**

```lua
function tick()
	--Offset the tool depending on character height
	local defaultEyeY = 1.7
	local offsetY = characterHeight - defaultEyeY
	local offset = Vec(0, offsetY, 0)
	SetToolOffset(offset)
end
```

---

### [API] SetPlayerOrientation

```lua
SetPlayerOrientation(orientation)
```

Sets the base orientation when gravity is disabled with SetGravity. This will determine what direction is "up", "right" and "forward" as gravity is completely turned off.

**Arguments:**

- `orientation` *(Quat)* — Base orientation

**Example:**

```lua
function tick()
	SetGravity(Vec(0, 0, 0))

	-- Turn player upside-down.
	local base = QuatAxisAngle(Vec(1,0,0), 180)
	SetPlayerOrientation(base)
end
```

---

### [API] GetPlayerOrientation

```lua
orientation = GetPlayerOrientation()
```

Gets the base orientation of the player. This can be used to retrieve the base orientation of the player when using a custom gravity vector.

**Example:**

```lua
function tick(dt)
	SetGravity(Vec(0, 0, 0))
	-- Spin the player if using zero gravity 
	local base = QuatRotateQuat(GetPlayerOrientation(), QuatAxisAngle(Vec(1,0,0), dt))
	SetPlayerOrientation(base)
end
```

---

### [API] GetPlayerUp

```lua
up = GetPlayerUp()
```

Returns the current "up" vector derived from the player's base orientation. This can be used to retrieve the player's up vector when using a custom gravity vector.

**Example:**

```lua
function tick(dt)
	local up = GetPlayerUp()
end
```

---


---
**Navigation:**
[Teardown scripting](00_overview.md) | [Parameters](01_parameters.md) | [Script control](02_script_control.md) | [Registry](03_registry.md) | [Vector math](04_vector_math.md) | [Entity](05_entity.md) | [Body](06_body.md) | [Shape](07_shape.md) | [Location](08_location.md) | [Joint](09_joint.md) | [Animation](10_animation.md) | [Light](11_light.md) | [Trigger](12_trigger.md) | [Screen](13_screen.md) | [Vehicle](14_vehicle.md) | **Player** | [Sound](16_sound.md) | [Sprite](17_sprite.md) | [Scene queries](18_scene_queries.md) | [Particles](19_particles.md) | [Spawning](20_spawning.md) | [Miscellaneous](21_miscellaneous.md) | [User Interface](22_ui.md)