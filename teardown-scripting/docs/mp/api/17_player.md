# Player

The player functions expose certain information about the player.

### [API] name = GetAllPlayers()
- **Returns:**
  - `name` *(list)* — List of all player Ids
```lua
local playerIds = GetAllPlayers()
```

### [API] count = GetMaxPlayers()
- **Returns:**
  - `count` *(interger)* — Number of max players for the session. Returns 1 for non-multiplayer.
```lua
local maxPlayerCount = GetMaxPlayers()
-- create an UI big enough to fit a the max player count
createGameModeUI(maxPlayerCount)
```

### [API] count = GetPlayerCount()
- **Returns:**
  - `count` *(number)* — Number of players
```lua
local playerCount = GetPlayerCount()
```

### [API] playerIds = GetAddedPlayers()
- **Returns:**
  - `playerIds` *(table)* — List of added player Ids
```lua
local playerIds = GetAddedPlayers()
```

### [API] playerIds = GetRemovedPlayers()
- **Returns:**
  - `playerIds` *(table)* — List of removed player Ids
```lua
local playerIds = GetRemovedPlayers()
```

### [API] name = GetPlayerName([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `name` *(string)* — Player name
```lua
local name = GetPlayerName(0)
```

### [API] GetLocalPlayer = GetLocalPlayer()
- **Returns:**
  - `GetLocalPlayer` *(number)* — Local player ID.
```lua
local p = GetLocalPlayer()
```

### [API] IsPlayerLocal = IsPlayerLocal([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `IsPlayerLocal` *(boolean)* — Whether a player is the local player.
```lua
if IsPlayerLocal(attacker) then
	score = score + 1
end
```

### [API] character = GetPlayerCharacter([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `character` *(string)* — Character id
```lua
local character = GetPlayerCharacter(0)
```

### [API] IsPlayerHost = IsPlayerHost([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `IsPlayerHost` *(boolean)* — Whether a player is the host
```lua
local isHost = IsPlayerHost()
```

### [API] IsPlayerValid = IsPlayerValid([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `IsPlayerValid` *(boolean)* — Whether a player is valid (existing player)
```lua
local isValid = IsPlayerValid(flagCarrier)
if not isValid then
	dropFlag()
end
```

### [API] position = GetPlayerPos([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `position` *(TVec)* — Player center position
```lua
function client.init()
	local p = GetPlayerPos()
	DebugPrint(p)

	--This is equivalent to
	p = VecAdd(GetPlayerTransform().pos, Vec(0,1,0))
	DebugPrint(p)
end
```

### [API] hit, startpos, endpos, direction, hitnormal, hitdist, hitentity, hitmaterial = GetPlayerAimInfo(position, [maxdist], [playerId])
- **Args:**
  - `position` *(TVec)* — Start position of the search
  - `maxdist` *(number, optional)* — Max search distance
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `hit` *(boolean)* — TRUE if hit, FALSE otherwise.
  - `startpos` *(TVec)* — Player can modify start position when close to walls etc
  - `endpos` *(TVec)* — Hit position
  - `direction` *(TVec)* — Direction from start position to end position
  - `hitnormal` *(TVec)* — Normal of the hitpoint
  - `hitdist` *(number)* — Distance of the hit
  - `hitentity` *(handle)* — Handle of the entitiy being hit
  - `hitmaterial` *(handle)* — Name of the material being hit
```lua
local muzzle = GetToolLocationWorldTransform("muzzle")
local _, pos, _, dir = GetPlayerAimInfo(muzzle.pos)
Shoot(pos, dir)
```

### [API] pitch = GetPlayerPitch([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `pitch` *(number)* — Current player pitch angle
```lua
function client.init()
	local pitchRotation = Quat(Vec(1,0,0), GetPlayerPitch())
end
```

### [API] yaw = GetPlayerYaw([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `yaw` *(number)* — Current player yaw angle
```lua
function client.init()
	local compassBearing = GetPlayerYaw()
end
```

### [API] SetPlayerPitch(pitch, [playerId])
- **Args:**
  - `pitch` *(number)* — Pitch.
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
```lua
function server.tick()
	-- look straight ahead
	SetPlayerPitch(0.0, playerId)
end
```

### [API] recoil = GetPlayerCrouch([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `recoil` *(number)* — Current player crouch
```lua
function client.tick()
    local crouch = GetPlayerCrouch()
    if crouch > 0.0 then
        ...
    end
end
```

### [API] transform = GetPlayerTransform([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `transform` *(TTransform)* — Current player transform
```lua
function client.init()
	local t = GetPlayerTransform()
	DebugPrint(TransformStr(t))
end
```

### [API] transform = GetPlayerTransformWithPitch([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `transform` *(table)* — Current player transform, including pitch (look up/down)
```lua
local t = GetPlayerTransform()
```

### [API] SetPlayerTransform(transform, [playerId])
- **Args:**
  - `transform` *(TTransform)* — Desired player transform
  - `playerId` *(number, optional)* — Player ID. On server, zero means server (host) player.
```lua
function server.tick()
	if InputPressed("jump", playerId) then
		local t = Transform(Vec(50, 0, 0), QuatEuler(0, 90, 0))
		SetPlayerTransform(t, playerId)
	end
end
```

### [API] SetPlayerTransformWithPitch(transform, [playerId])
- **Args:**
  - `transform` *(table)* — Desired player transform
  - `playerId` *(number, optional)* — Player ID. On server, zero means server (host) player.
```lua
local t = Transform(Vec(10, 0, 0), QuatEuler(30, 90, 0))
SetPlayerTransform(t, playerId)
```

### [API] SetPlayerGroundVelocity(vel, [playerId])
- **Args:**
  - `vel` *(TVec)* — Desired ground velocity
  - `playerId` *(number, optional)* — Player ID. On server, zero means server (host) player.
```lua
function server.tick()
	SetPlayerGroundVelocity(Vec(2,0,0), playerId)
end
```

### [API] transform = GetPlayerEyeTransform([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `transform` *(TTransform)* — Current player eye transform
```lua
function client.init()
	local t = GetPlayerEyeTransform()
	DebugPrint(TransformStr(t))
end
```

### [API] transform = GetPlayerCameraTransform([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `transform` *(TTransform)* — Current player camera transform
```lua
function client.init()
	local t = GetPlayerCameraTransform()
	DebugPrint(TransformStr(t))
end
```

### [API] SetPlayerCameraOffsetTransform(transform, [stackable], [playerId])
- **Args:**
  - `transform` *(TTransform)* — Desired player camera offset transform
  - `stackable` *(boolean, optional)* — True if eye offset should summ up with multiple calls per tick
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player.
```lua
function client.tick()
	local t = Transform(Vec(), QuatAxisAngle(Vec(1, 0, 0), math.sin(GetTime()*3.0) * 3.0))
	SetPlayerCameraOffsetTransform(t, playerId)
end
```

### [API] SetPlayerSpawnTransform(transform, [playerId])
- **Args:**
  - `transform` *(TTransform)* — Desired player spawn transform
  - `playerId` *(number, optional)* — Player ID. On server, zero means server (host) player.
```lua
function setPlayerSpawnTransform(playerId)
	local t = Transform(Vec(10, 0, 0), QuatEuler(0, 90, 0))
	SetPlayerSpawnTransform(t, playerId)
end
```

### [API] SetPlayerSpawnHealth(health, [playerId])
- **Args:**
  - `health` *(number)* — Desired player spawn health (between zero and one)
  - `playerId` *(number, optional)* — Player ID. On server, zero means server (host) player.
```lua
function playerJoined(playerId)
	SetPlayerSpawnHealth(0.5, playerId)
end
```

### [API] SetPlayerSpawnTool(id, [playerId])
- **Args:**
  - `id` *(string)* — Tool unique identifier
  - `playerId` *(number, optional)* — Player ID. On server, zero means server (host) player.
```lua
function playerJoined(playerId)
	SetPlayerSpawnTool("pistol", playerId)
end
```

### [API] velocity = GetPlayerVelocity([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `velocity` *(TVec)* — Player velocity in world space as vector
```lua
function client.tick()
	local vel = GetPlayerVelocity()
	DebugPrint(VecStr(vel))
end
```

### [API] SetPlayerVehicle(vehicle, [playerId])
- **Args:**
  - `vehicle` *(number)* — Handle to vehicle or zero to not drive.
  - `playerId` *(number, optional)* — Player ID. On server, zero means server (host) player.
```lua
function server.tick()
	if InputPressed("interact", playerId) then
		local car = FindVehicle("mycar")
		SetPlayerVehicle(car, playerId)
	end
end
```

### [API] SetPlayerAnimator(animator, [playerId])
- **Args:**
  - `animator` *(number)* — Handle to animator or zero for no animator
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.

### [API] animator = GetPlayerAnimator([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `animator` *(number)* — Handle to animator or zero for no animator

### [API] bodies = GetPlayerBodies([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `bodies` *(list)* — Get bodies associated with a player
```lua
local bodies = GetPlayerBodies(playerId)
```

### [API] SetPlayerVelocity(velocity, [playerId])
- **Args:**
  - `velocity` *(TVec)* — Player velocity in world space as vector
  - `playerId` *(number, optional)* — Player ID. On server, zero means server (host) player.
```lua
function server.tick()
	if InputPressed("jump", playerId) then
		SetPlayerVelocity(Vec(0, 5, 0), playerId)
	end
end
```

### [API] handle = GetPlayerVehicle([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `handle` *(number)* — Current vehicle handle, or zero if not in vehicle
```lua
function client.tick()
	local vehicle = GetPlayerVehicle()
	if vehicle ~= 0 then
		DebugPrint("Player drives the vehicle")
	end
end
```

### [API] isGrounded = IsPlayerGrounded([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `isGrounded` *(boolean)* — Whether the player is grounded
```lua
local isGrounded = IsPlayerGrounded()
```

### [API] isDriver = IsPlayerVehicleDriver(handle, [playerId])
- **Args:**
  - `handle` *(number)* — Vehicle handle
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `isDriver` *(boolean)* — Whether the player is driver for this vehicle
```lua
local vehicle = FindVehicle("myvehicle")
local isDriver = IsPlayerVehicleDriver(vehicle)
```

### [API] isPassenger = IsPlayerVehiclePassenger(handle, [playerId])
- **Args:**
  - `handle` *(number)* — Vehicle handle
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `isPassenger` *(boolean)* — Whether the player is a passenger of this vehicle
```lua
local vehicle = FindVehicle("myvehicle")
local isPassenger = IsPlayerVehiclePassenger(vehicle)
```

### [API] isGrounded = IsPlayerJumping([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `isGrounded` *(boolean)* — Whether the player is jumping or not
```lua
local isJumping = IsPlayerJumping()
```

### [API] contact, shape, point, normal = GetPlayerGroundContact([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `contact` *(boolean)* — Whether the player is grounded
  - `shape` *(number)* — Handle to shape
  - `point` *(Vec)* — Point of contact
  - `normal` *(Vec)* — Normal of contact
```lua
function client.tick()
	hasGroundContact, shape, point, normal = GetPlayerGroundContact()

	if hasGroundContact then
		-- print ground contact data
		DebugPrint(VecStr(point).." : "..VecStr(normal))
	end
end
```

### [API] handle = GetPlayerGrabShape([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `handle` *(number)* — Handle to grabbed shape or zero if not grabbing.
```lua
function client.tick()
	local shape = GetPlayerGrabShape()
	if shape ~= 0 then
		DebugPrint("Player is grabbing a shape")
	end
end
```

### [API] handle = GetPlayerGrabBody([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `handle` *(number)* — Handle to grabbed body or zero if not grabbing.
```lua
function client.tick()
	local body = GetPlayerGrabBody()
	if body ~= 0 then
		DebugPrint("Player is grabbing a body")
	end
end
```

### [API] ReleasePlayerGrab([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On server, zero means server (host) player.
```lua
function server.tick()
	if InputPressed("jump", playerId) then
		ReleasePlayerGrab(playerId)
	end
end
```

### [API] pos = GetPlayerGrabPoint([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `pos` *(TVec)* — The world space grab point.
```lua
local body = GetPlayerGrabBody()
if body ~= 0 then
	local pos = GetPlayerGrabPoint()
end
```

### [API] handle = GetPlayerPickShape([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `handle` *(number)* — Handle to picked shape or zero if nothing is picked
```lua
function client.tick()
	local shape = GetPlayerPickShape()
	if shape ~= 0 then
		DebugPrint("Picked shape " .. shape)
	end
end
```

### [API] handle = GetPlayerPickBody([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `handle` *(number)* — Handle to picked body or zero if nothing is picked
```lua
function client.tick()
	local body = GetPlayerPickBody()
	if body ~= 0 then
		DebugWatch("Pick body ", body)
	end
end
```

### [API] handle = GetPlayerInteractShape([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `handle` *(number)* — Handle to interactable shape or zero
```lua
function client.tick()
	local shape = GetPlayerInteractShape()
	if shape ~= 0 then
		DebugPrint("Interact shape " .. shape)
	end
end
```

### [API] handle = GetPlayerInteractBody([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `handle` *(number)* — Handle to interactable body or zero
```lua
function client.tick()
	local body = GetPlayerInteractBody()
	if body ~= 0 then
		DebugPrint("Interact body " .. body)
	end
end
```

### [API] SetPlayerScreen(handle, [playerId])
- **Args:**
  - `handle` *(number)* — Handle to screen or zero for no screen
  - `playerId` *(number, optional)* — Player ID. On server, zero means server (host) player.
```lua
function server.tick()
	if InputPressed("interact", playerId) then
		if GetPlayerScreen(playerId) ~= 0 then
			SetPlayerScreen(0, playerId)
		else
			SetPlayerScreen(screen, playerId)
		end

	end
end
```

### [API] handle = GetPlayerScreen([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `handle` *(number)* — Handle to interacted screen or zero if none
```lua
function server.tick()
	if InputPressed("interact", playerId) then
		if GetPlayerScreen(playerId) ~= 0 then
			SetPlayerScreen(0, playerId)
		else
			SetPlayerScreen(screen, playerId)
		end

	end
end
```

### [API] SetPlayerHealth(health, [playerId])
- **Args:**
  - `health` *(number)* — Set player health (between zero and one)
  - `playerId` *(number, optional)* — Player ID. On server, zero means server (host) player.
```lua
function server.tick()
	if InputPressed("interact", playerId) then
		if GetPlayerHealth() < 0.75 then
			SetPlayerHealth(1.0, playerId)
		else
			SetPlayerHealth(0.5, playerId)
		end
	end
end
```

### [API] health = GetPlayerHealth([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `health` *(number)* — Current player health
```lua
function server.tick()
	if InputPressed("interact", playerId) then
		if GetPlayerHealth() < 0.75 then
			SetPlayerHealth(1.0, playerId)
		else
			SetPlayerHealth(0.5, playerId)
		end
	end
end
```

### [API] canusetool = GetPlayerCanUseTool([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `canusetool` *(bool)* — If the player currenty can use tool.
```lua
function server.tick()
	for p in Players() do
		if GetPlayerCanUseTool(p) and InputPressed("usetool", p) then
			-- fire laser
		end
	end
end
```

### [API] SetPlayerRegenerationState(state, [player])
- **Args:**
  - `state` *(boolean)* — State of player regeneration
  - `player` *(number, optional)* — Player ID change regeneration for
```lua
function playerJoined(playerId)
	-- initially disable regeneration for player
	SetPlayerRegenerationState(false, playerId)
end
```

### [API] SetPlayerTool(tool id, [playerId])
- **Args:**
  - `tool id` *(string)* — Set Tool ID
  - `playerId` *(number, optional)* — Player ID. On server, zero means server (host) player.
```lua
function playerJoined(playerId)
	-- Server sets player tool to "gun"
	SetPlayerTool("gun", playerId)
end
```

### [API] tool id = GetPlayerTool([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `tool id` *(string)* — Get Tool ID
```lua
local tool = GetPlayerTool()
```

### [API] RespawnPlayer([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On server, zero means server (host) player.
```lua
function server.tick()
	for p in Players() do
		if InputPressed("interact", p) then
			RespawnPlayer(p)
		end
	end
end
```

### [API] RespawnPlayerAtTransform(transform, [playerId])
- **Args:**
  - `transform` *(transform)* — Transform
  - `playerId` *(number, optional)* — Player ID. On server, zero means server (host) player.
```lua
function server.tick()
	for p in Players() do
		if InputPressed("interact", p) then
			RespawnPlayerAtTransform(Transform(Vec(1,2,3)), p)
		end
	end
end
```

### [API] speed = GetPlayerWalkingSpeed([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `speed` *(number)* — Current player base walking speed
```lua
function client.tick()
	DebugPrint(GetPlayerWalkingSpeed())
end
```

### [API] SetPlayerWalkingSpeed(speed, [playerId])
- **Args:**
  - `speed` *(number)* — Set player walking speed
  - `playerId` *(number, optional)* — Player ID. On server, zero means server (host) player.
```lua
function server.tick()

	for p in Players() do
		-- Set player walking speed based on whether shift is pressed
		if InputDown("shift", p) then
			SetPlayerWalkingSpeed(15.0, p)
		else
			SetPlayerWalkingSpeed(7.0, p)
		end
	end
end
```

### [API] speed = GetPlayerCrouchSpeedScale([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `speed` *(number)* — Current player walking speed while crouched
```lua
function client.tick()
	DebugPrint(GetPlayerCrouchSpeedScale())
end
```

### [API] SetPlayerCrouchSpeedScale(speed, [playerId])
- **Args:**
  - `speed` *(number)* — Set player walking speed while crouched
  - `playerId` *(number, optional)* — Player ID. On server, zero means server (host) player.
```lua
function server.tick()
	for p in Players() do
		if InputDown("shift") then
			SetPlayerCrouchSpeedScale(5.0, p)
		else
			SetPlayerCrouchSpeedScale(3.0, p)
		end
	end
end
```

### [API] speed = GetPlayerHurtSpeedScale([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `speed` *(number)* — Current player walking speed when hurt
```lua
function client.tick()
	DebugPrint(GetPlayerHurtSpeedScale())
end
```

### [API] SetPlayerHurtSpeedScale(speed, [playerId])
- **Args:**
  - `speed` *(number)* — Set player walking speed when hurt
  - `playerId` *(number, optional)* — Player ID. On server, zero means server (host) player.
```lua
function server.tick()
	-- Reduce hurt penalty (default is 2/7 or roughly 0.29)
	for p in Players() do
		SetPlayerHurtSpeedScale(0.6, p)
	end
end
```

### [API] value = GetPlayerParam(parameter, [player])
- **Args:**
  - `parameter` *(string)* — Parameter name
  - `player` *(number, optional)* — Player ID. On player, zero means local player.
- **Returns:**
  - `value` *(any)* — Parameter value
| Param name | Type | Description |
| --- | --- | --- |
| health | float | Current value of the player's health. |
| healthRegeneration | boolean | Is the player's health regeneration enabled. |
| walkingSpeed | float | The player's walking speed. |
| jumpSpeed | float | The player's jump speed. |
| godMode | boolean | If the value is True, the player does not lose health |
| friction | float | Player body friction |
| frictionMode | string | Player friction combine mode |
| flyMode | boolean | If the value is True, the player will fly |
| flashlightAllowed | boolean | Changes ability to use flashlight |
| disableInteract | boolean | Disable interactions for player |
| CollisionMask | int | Player collision mask bits (0-255) with respect to all shapes layer bits |
```lua
function client.tick()
	-- The parameter names are case-insensitive, so any of the specified writing styles will be correct:
	-- "GodMode", "godmode", "godMode"
	local paramName = "GodMode"
	local param = GetPlayerParam(paramName)
	DebugWatch(paramName, param)
end
```

### [API] SetPlayerParam(parameter, value, [player])
- **Args:**
  - `parameter` *(string)* — Parameter name
  - `value` *(any)* — Parameter value
  - `player` *(number, optional)* — Player ID. On player, zero means local player.
| Param name | Type | Description |
| --- | --- | --- |
| health | float | Current value of the player's health. |
| healthRegeneration | boolean | Is the player's health regeneration enabled. |
| walkingSpeed | float | The player's walking speed. This value is applied for 1 frame! |
| jumpSpeed | float | The player's jump speed. The height of the jump depends non-linearly on the jump speed. This value is applied for 1 frame! |
| godMode | boolean | If the value is True, the player does not lose health |
| friction | float | Player body friction. Default is 0.8 |
| frictionMode | string | Player friction combine mode. Can be (average\|minimum\|multiply\|maximum) |
| flyMode | boolean | If the value is True, the player will fly |
| flashlightAllowed | boolean | Changes ability to use flashlight |
| disableInteract | boolean | Disable interactions for player |
| CollisionMask | int | Player collision mask bits (0-255) with respect to all shapes layer bits |
```lua
function server.tick()
	-- The parameter names are case-insensitive, so any of the specified writing styles will be correct:
	-- "JumpSpeed", "jumpspeed", "jumpSpeed"
	local paramName = "JumpSpeed"

	for p in Players() do
		-- Set player jump speed based on whether shift is pressed
		if InputDown("shift", p) then
			SetPlayerParam(paramName, 10, p)
		else
			SetPlayerParam(paramName, 5, p)
		end
	end
end
```

### [API] SetPlayerHidden([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
```lua
function client.tick()
	...
	SetCameraTransform(t)
	SetPlayerHidden()
end
```

### [API] RegisterTool(id, name, file, [group])
- **Args:**
  - `id` *(string)* — Tool unique identifier
  - `name` *(string)* — Tool name to show in hud
  - `file` *(string)* — Path to vox file or prefab xml
  - `group` *(number, optional)* — Tool group for this tool (1-6) Default is 6.
```lua
function server.init()
	RegisterTool("lasergun", "Laser Gun", "MOD/vox/lasergun.vox", 6)
end

function server.tick()
	for p in Players() do
		if GetPlayerTool(p) == "lasergun" then
			--Tool is selected. Tool logic goes here.

			if InputPressed("usetool", p) then
				-- Fire the tool
			end
		end
	end
end

function client.tick()
	for p in Players() do
		if GetPlayerTool(p) == "lasergun" then
			if InputPressed("usetool", p) then
				-- Spawn client side particles, play sound, etc.
			end
		end
	end
end
```

### [API] SetToolAmmoPickupAmount(toolId, ammo)
- **Args:**
  - `toolId` *(string)* — Tool ID
  - `ammo` *(number)* — The default ammo pickup amount
```lua
function server.init()
	RegisterTool("lasergun", "Laser Gun", "MOD/vox/lasergun.vox", 6)
	SetToolAmmoPickupAmount("lasergun", 30)
end
```

### [API] ammo = GetToolAmmoPickupAmount(toolId)
- **Args:**
  - `toolId` *(string)* — Tool ID
- **Returns:**
  - `ammo` *(number)* — The default ammo pickup amount
```lua
local ammo = GetToolAmmoPickupAmount("gun")
```

### [API] handle = GetToolBody([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `handle` *(number)* — Handle to currently visible tool body or zero if none
```lua
function client.tick()
	local toolBody = GetToolBody()
	if toolBody~=0 then
		DebugPrint("Tool body: " .. toolBody)
	end
end
```

### [API] right, left = GetToolHandPoseLocalTransform([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `right` *(TTransform)* — Transform of right hand relative to the tool body origin, or nil if the right hand is not used
  - `left` *(TTransform)* — Transform of left hand, or nil if left hand is not used
```lua
local right, left = GetToolHandPoseLocalTransform()
```

### [API] right, left = GetToolHandPoseWorldTransform([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `right` *(TTransform)* — Transform of right hand in world space, or nil if the right hand is not used
  - `left` *(TTransform)* — Transform of left hand, or nil if left hand is not used
```lua
local right, left = GetToolHandPoseWorldTransform()
```

### [API] SetToolHandPoseLocalTransform(right, left, [playerId])
- **Args:**
  - `right` *(TTransform)* — Transform of right hand relative to the tool body origin, or nil if right hand is not used
  - `left` *(TTransform)* — Transform of left hand, or nil if left hand is not used
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player.
```lua
if GetBool("game.thirdperson") then
	if aiming then
		SetToolHandPoseLocalTransform(Transform(Vec(0.2,0.0,0.0), QuatAxisAngle(Vec(0,1,0), 90.0)), Transform(Vec(-0.1, 0.0, -0.4)))
	else
		SetToolHandPoseLocalTransform(Transform(Vec(0.2,0.0,0.0), QuatAxisAngle(Vec(0,1,0), 90.0)), nil)
	end
end
```

### [API] location = GetToolLocationLocalTransform(name, [playerId])
- **Args:**
  - `name` *(string)* — Name of location
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `location` *(TTransform)* — Transform of a tool location in tool space or nil if location is not found.
```lua
local right  = GetToolLocationLocalTransform("righthand")
SetToolHandPoseLocalTransform(right, nil)
```

### [API] location = GetToolLocationWorldTransform(name, [playerId])
- **Args:**
  - `name` *(string)* — Name of location
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `location` *(TTransform)* — Transform of a tool location in world space or nil if the location is not found or if there is no visible tool body.
```lua
local muzzle = GetToolLocationWorldTransform("muzzle")
Shoot(muzzle, direction)
```

### [API] SetToolTransform(transform, [sway], [playerId])
- **Args:**
  - `transform` *(TTransform)* — Tool body transform
  - `sway` *(number, optional)* — Tool sway amount. Default is 1.0
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player.
```lua
function client.tick()
	--Offset the tool half a meter to the right for the local player
	local offset = Transform(Vec(0.5, 0, 0))
	SetToolTransform(offset)
end
```

### [API] SetToolAllowedZoom(zoom, [zoom sensitivity])
- **Args:**
  - `zoom` *(number)* — Zoom factor
  - `zoom sensitivity` *(number, optional)* — Input sensitivity when zoomed in. Default is 1.0.
```lua
function client.tick()
	-- allow our scoped tool to zoom by factor 4.
	SetToolAllowedZoom(4.0, 0.5)
end
```

### [API] SetToolTransformOverride(transform, [playerId])
- **Args:**
  - `transform` *(TTransform)* — Tool body transform
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player.
```lua
function client.tick()

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

### [API] SetToolOffset(offset, [playerId])
- **Args:**
  - `offset` *(TVec)* — Tool body offset
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player.
```lua
function client.tick()
	--Offset the tool depending on character height
	local defaultEyeY = 1.7
	local offsetY = characterHeight - defaultEyeY
	local offset = Vec(0, offsetY, 0)
	SetToolOffset(offset)
end
```

### [API] SetToolAmmo(toolId, ammo, [playerId])
- **Args:**
  - `toolId` *(string)* — Tool ID
  - `ammo` *(number)* — Total ammo
  - `playerId` *(number, optional)* — Player ID. On server, zero means server (host) player.
```lua
SetToolAmmo("gun", 10, 1)
```

### [API] ammo = GetToolAmmo(toolId, [playerId])
- **Args:**
  - `toolId` *(string)* — Tool ID
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `ammo` *(number)* — Total ammo for tool
```lua
local ammo = GetToolAmmo("gun", 1)
```

### [API] SetToolEnabled(toolId, enabled, [playerId])
- **Args:**
  - `toolId` *(string)* — Tool ID
  - `enabled` *(bool)* — Tool enabled
  - `playerId` *(number, optional)* — Player ID
```lua
SetToolEnabled("gun", false, playerId)
```

### [API] enabled = IsToolEnabled(toolId, [playerId])
- **Args:**
  - `toolId` *(string)* — Tool ID
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `enabled` *(bool)* — Tool enabled for player
```lua
if IsToolEnabled("gun", 1) then
	...
end
```

### [API] SetPlayerOrientation(orientation, [playerId])
- **Args:**
  - `orientation` *(Quat)* — Base orientation
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
```lua
function server.tick()
	SetGravity(Vec(0, 0, 0))

	-- Turn players upside-down.
	for p in Players() do
		SetPlayerOrientation(QuatAxisAngle(Vec(1,0,0), 180), p)
	end
end
```

### [API] GetPlayerOrientation([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
```lua
function server.tick(dt)
	SetGravity(Vec(0, 0, 0))

	for p in Players() do
		-- Spin the player if using zero gravity
		local base = QuatRotateQuat(GetPlayerOrientation(p), QuatAxisAngle(Vec(1,0,0), dt))
		SetPlayerOrientation(base, p)
	end
end
```

### [API] up = GetPlayerUp([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `up` *(TVec)* — Up vector of the player
```lua
function client.tick()
	local up = GetPlayerUp()
	DebugPrint("Player up vector: " .. up)
end
```

### [API] SetPlayerRig(rig, [playerId])
- **Args:**
  - `rig` *(number)* — Rig handle
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
```lua
    local rig = FindRig("myrig")
    SetPlayerRig(rig)
```

### [API] rig = GetPlayerRig([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `rig` *(number)* — Rig handle
```lua
local rig = GetPlayerRig(rigid)
```

### [API] transform = GetPlayerRigWorldTransform([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `transform` *(TTransform)* — World transform, nil if player doesnt have a rig
```lua
local t = GetPlayerRigWorldTransform()
```

### [API] ClearPlayerRig(rig-id, [playerId])
- **Args:**
  - `rig-id` *(number)* — Unique rig-id, -1 means all rigs
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
```lua
    ClearPlayerRig(someId)
```

### [API] SetPlayerRigLocationLocalTransform(rig-id, name, location, [playerId])
- **Args:**
  - `rig-id` *(number)* — Unique id
  - `name` *(string)* — Name of location
  - `location` *(table)* — Rig Local transform of the location
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
```lua
    local someBody = FindBody("bodyname")
    SetPlayerRigLocationLocalTransform(someBody, "ik_foot_l", TransformToLocalTransform(GetBodyTransform(someBody), GetLocationTransform(FindLocation("ik_foot_l"))))
```

### [API] SetPlayerRigTransform(rig-id, location, [playerId])
- **Args:**
  - `rig-id` *(number)* — Unique id
  - `location` *(table)* — New world transform
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
```lua
    local someBody = FindBody("bodyname")
    SetPlayerRigTransform(someBody, GetBodyTransform(someBody))
```

### [API] location = GetPlayerRigLocationWorldTransform(name, [playerId])
- **Args:**
  - `name` *(string)* — Name of location
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `location` *(table)* — Transform of a location in world space
```lua
local t = GetPlayerRigLocationWorldTransform("ik_hand_l")
```

### [API] SetPlayerRigTags(rig-id, tag, [playerId])
- **Args:**
  - `rig-id` *(number)* — Unique id
  - `tag` *(string)* — Tag
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player.

### [API] exists = GetPlayerRigHasTag(tag, [playerId])
- **Args:**
  - `tag` *(string)* — Tag name
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `exists` *(boolean)* — Returns true if entity has tag

### [API] value = GetPlayerRigTagValue(tag, [playerId])
- **Args:**
  - `tag` *(string)* — Tag name
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `value` *(string)* — Returns the tag value, if any. Empty string otherwise.

### [API] inuse, r, g, b = GetPlayerColor([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `inuse` *(boolean)* — If color is used or not
  - `r` *(number)* — Red channel value
  - `g` *(number)* — Green channel value
  - `b` *(number)* — Blue channel value
```lua
function client.tick()
	local inuse, r, g, b = GetPlayerColor()
	if inuse then
		DebugPrint("Player color: " .. r .. ", " .. g .. ", " .. b)
	else
		DebugPrint("Player color is not set")
	end
end
```

### [API] SetPlayerColor(r, g, b, [playerId])
- **Args:**
  - `r` *(number)* — Red value
  - `g` *(number)* — Green value
  - `b` *(number)* — Blue value
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
```lua
end
function client.tick()
	local r, g, b = 1.0, 0.5, 0.2
	SetPlayerColor(r, g, b)
	DebugPrint("Set player color to: " .. r .. ", " .. g .. ", " .. b)
end
```

### [API] ApplyPlayerDamage(targetPlayerId, damage, [cause], [instigatingPlayerId])
- **Args:**
  - `targetPlayerId` *(number)* — Target player ID
  - `damage` *(number)* — Damage to apply to target player
  - `cause` *(string, optional)* — The cause of damage
  - `instigatingPlayerId` *(number, optional)* — Instigating player ID.
```lua
function server.tick(dt)
	
	for player in Players() do
		if isOnFire(player) then
			-- Apply 20% of dt as damage to the player
			ApplyPlayerDamage(player, 0.2 * dt, "fire")
		end
	end
	
	-- or

	for player in Players() do
		if InputIsPressed("usetool", player) then
			for target in Players() do
				if target ~= player and isInRange(player, target) then
					-- Apply 50% damage to the target player
					ApplyPlayerDamage(target, 0.5, "tool", player)
				end
			end
		end
	end
end
```

### [API] DisablePlayerInput(player)
- **Args:**
  - `player` *(playerIndex)* — Player to disable input for
```lua
-- Disable player 2 input as she/he is interacting with something.
DisablePlayerInput(2)
```

### [API] DisablePlayer(playerId)
- **Args:**
  - `playerId` *(number)* — Player to disable
```lua
function updateFinalScoreboard()
	for i=1,#hiddenPlayers do
		DisablePlayer(hiddenPlayers[i])
	end
end
```

### [API] IsPlayerDisabled(playerId)
- **Args:**
  - `playerId` *(number)* — Check if player is disabled
```lua
--check if disabled
playerDisabled = IsPlayerDisabled(playerId)
```

### [API] DisablePlayerDamage(playerId)
- **Args:**
  - `playerId` *(number)* — Player for which damage should be disabled
```lua
function server.tick()
	for i=1,#invulnerablePlayers do
		DisablePlayerDamage(invulnerablePlayers[i])
	end
end
```

---
**Navigation:** [_INDEX](_INDEX.md)