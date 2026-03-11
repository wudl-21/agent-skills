# Vehicle

Vehicles are set up in the editor and consists of multiple parts owned by a vehicle entity.

### [API] handle = FindVehicle([tag], [global])
- **Args:**
  - `tag` *(string, optional)* — Tag name
  - `global` *(boolean, optional)* — Search in entire scene
- **Returns:**
  - `handle` *(number)* — Handle to first vehicle with specified tag or zero if not found
```lua
function client.init()
	local vehicle = FindVehicle("mycar")
end
```

### [API] list = FindVehicles([tag], [global])
- **Args:**
  - `tag` *(string, optional)* — Tag name
  - `global` *(boolean, optional)* — Search in entire scene
- **Returns:**
  - `list` *(table)* — Indexed table with handles to all vehicles with specified tag
```lua
function client.init()
	--Find all vehicles in level tagged "boat"
	local boats = FindVehicles("boat")
	for i=1, #boats do
		local boat = boats[i]
		DebugPrint(boat)
	end
end
```

### [API] transform = GetVehicleTransform(vehicle)
- **Args:**
  - `vehicle` *(number)* — Vehicle handle
- **Returns:**
  - `transform` *(TTransform)* — Transform of vehicle
```lua
function client.init()
	local vehicle = FindVehicle("vehicle")
	local t = GetVehicleTransform(vehicle)
end
```

### [API] transforms = GetVehicleExhaustTransforms(vehicle)
- **Args:**
  - `vehicle` *(number)* — Vehicle handle
- **Returns:**
  - `transforms` *(table)* — Transforms of vehicle exhausts
```lua
function client.tick()
	local vehicle = FindVehicle("car", true)
	local t = GetVehicleExhaustTransforms(vehicle)
	for i = 1, #t do
		DebugWatch(tostring(i), t[i])
	end
end
```

### [API] transforms = GetVehicleVitalTransforms(vehicle)
- **Args:**
  - `vehicle` *(number)* — Vehicle handle
- **Returns:**
  - `transforms` *(table)* — Transforms of vehicle vitals
```lua
function client.tick()
	local vehicle = FindVehicle("car", true)
	local t = GetVehicleVitalTransforms(vehicle)
	for i = 1, #t do
		DebugWatch(tostring(i), t[i])
	end
end
```

### [API] transforms = GetVehicleBodies(vehicle)
- **Args:**
  - `vehicle` *(number)* — Vehicle handle
- **Returns:**
  - `transforms` *(table)* — Vehicle bodies handles
```lua
function client.tick()
	local vehicle = FindVehicle("car", true)
	local t = GetVehicleBodies(vehicle)
	for i = 1, #t do
		DebugWatch(tostring(i), t[i])
	end
end
```

### [API] body = GetVehicleBody(vehicle)
- **Args:**
  - `vehicle` *(number)* — Vehicle handle
- **Returns:**
  - `body` *(number)* — Main body of vehicle
```lua
function client.init()
	local vehicle = FindVehicle("vehicle")
	local body = GetVehicleBody(vehicle)
	if IsBodyBroken(body) then
		DebugPrint("Is broken")
	end
end
```

### [API] health = GetVehicleHealth(vehicle)
- **Args:**
  - `vehicle` *(number)* — Vehicle handle
- **Returns:**
  - `health` *(number)* — Vehicle health (zero to one)
```lua
function client.init()
	local vehicle = FindVehicle("vehicle")
	local health = GetVehicleHealth(vehicle)
	DebugPrint(health)
end
```

### [API] params = GetVehicleParams(vehicle)
- **Args:**
  - `vehicle` *(number)* — Vehicle handle
- **Returns:**
  - `params` *(table)* — Vehicle params
```lua
function client.tick()
	local params = GetVehicleParams(FindVehicle("car", true))
	for key, value in pairs(params) do
		DebugWatch(key, value)
	end
end
```

### [API] SetVehicleParam(handle, param, value)
- **Args:**
  - `handle` *(number)* — Vehicle handler
  - `param` *(string)* — Param name
  - `value` *(number)* — Param value
```lua
function server.init()
	SetVehicleParam(FindVehicle("car", true), "topspeed", 200)
end
```

### [API] pos = GetVehicleDriverPos(vehicle)
- **Args:**
  - `vehicle` *(number)* — Vehicle handle
- **Returns:**
  - `pos` *(TVec)* — Driver position as vector in vehicle space
```lua
function client.init()
	local vehicle = FindVehicle("vehicle")
	local driverPos = GetVehicleDriverPos(vehicle)
	local t = GetVehicleTransform(vehicle)
	local worldPos = TransformToParentPoint(t, driverPos)
	DebugPrint(worldPos)
end
```

### [API] pos = GetVehicleAvailableSeatPos(vehicle)
- **Args:**
  - `vehicle` *(number)* — Vehicle handle
- **Returns:**
  - `pos` *(TVec)* — World space position of the next available seat. {0, 0, 0} if none is available.
```lua
function client.tick()
	local vehicle = FindVehicle("vehicle")
	local pos = GetVehicleAvailableSeatPos(vehicle)
	DebugPrint(pos)
end
```

### [API] steering = GetVehicleSteering(vehicle)
- **Args:**
  - `vehicle` *(number)* — Vehicle handle
- **Returns:**
  - `steering` *(number)* — Driver steering value -1 to 1
```lua
local steering = GetVehicleSteering(vehicle)
```

### [API] drive = GetVehicleDrive(vehicle)
- **Args:**
  - `vehicle` *(number)* — Vehicle handle
- **Returns:**
  - `drive` *(number)* — Driver drive value -1 to 1
```lua
local drive = GetVehicleDrive(vehicle)
```

### [API] DriveVehicle(vehicle, drive, steering, handbrake)
- **Args:**
  - `vehicle` *(number)* — Vehicle handle
  - `drive` *(number)* — Reverse/forward control -1 to 1
  - `steering` *(number)* — Left/right control -1 to 1
  - `handbrake` *(boolean)* — Handbrake control
```lua
function server.tick()
	--Drive mycar forwards
	local v = FindVehicle("mycar")
	DriveVehicle(v, 1, 0, false)
end
```

### [API] transform = GetVehicleLocationWorldTransform(vehicle, name)
- **Args:**
  - `vehicle` *(number)* — Vehicle handle
  - `name` *(string)* — Name of location
- **Returns:**
  - `transform` *(TTransform)* — World transform
```lua
local t = GetVehicleLocationWorldTransform(vehicle, "player_steeringwheel")
```

### [API] count, seats, hasDriver = GetVehiclePassengerCount(vehicle)
- **Args:**
  - `vehicle` *(number)* — Vehicle handle
- **Returns:**
  - `count` *(number)* — Number of passengers
  - `seats` *(number)* — Number of seats
  - `hasDriver` *(bool)* — If vehicle has a driver
```lua
local passengers, seats, hasDriver = GetVehiclePassengerCount(vehicle)
```

### [API] SetVehicleHealth(vehicle, health)
- **Args:**
  - `vehicle` *(number)* — Vehicle handle
  - `health` *(number)* — Set vehicle health (between zero and one)
```lua
function server.tick()
	if InputPressed("usetool", playerId) then
		SetVehicleHealth(FindVehicle("car", true), 0.0)
	end
end
```

---
**Navigation:** [_INDEX](_INDEX.md)