# Location

Locations are transforms placed in the editor as markers. Location transforms are always expressed in
world space coordinates.

### [API] handle = FindLocation([tag], [global])
- **Args:**
  - `tag` *(string, optional)* — Tag name
  - `global` *(boolean, optional)* — Search in entire scene
- **Returns:**
  - `handle` *(number)* — Handle to first location with specified tag or zero if not found
```lua
local loc = 0
function client.init()
	loc = FindLocation("loc1")
end

function client.tick()
	DebugCross(GetLocationTransform(loc).pos)
end
```

### [API] list = FindLocations([tag], [global])
- **Args:**
  - `tag` *(string, optional)* — Tag name
  - `global` *(boolean, optional)* — Search in entire scene
- **Returns:**
  - `list` *(table)* — Indexed table with handles to all locations with specified tag
```lua
local locations
function client.init()
	locations = FindLocations("loc1")

	for i=1, #locations do
		local loc = locations[i]
		DebugPrint(DebugPrint(loc))
	end
end
```

### [API] transform = GetLocationTransform(handle)
- **Args:**
  - `handle` *(number)* — Location handle
- **Returns:**
  - `transform` *(TTransform)* — Transform of the location
```lua
local location = 0
function client.init()
	location = FindLocation("loc1")
	DebugPrint(VecStr(GetLocationTransform(location).pos))
end
```

---
**Navigation:** [_INDEX](_INDEX.md)