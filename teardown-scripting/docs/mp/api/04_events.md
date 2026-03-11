# Events

- Scripts register listeners for events and trigger them for inter-script communication.
- The game engine also fires built-in events (player death, explosion, etc.).

## Built-in Events

| Event | Description | Parameters | Availability |
| --- | --- | --- | --- |
| playerhurt | Triggered when a player is hurt. | playerId (number), healthBefore (number), healthAfter (number), attackerId (number), point (TVec), impulse (TVec) | Server and Client |
| playerdied | Triggered when a player dies. | playerId (number), attackerId (number), damage (number), healthBefore (number), cause (string), point (TVec), impulse (TVec) | Server and Client |
| explosion | Triggered when an explosion occurs. | point (TVec), strength (number) | Server only |
| projectilehit | Triggered when a projectile hits an object. | shape (number), point (TVec), direction (TVec) | Server only |

## Functions

### [Event] value = GetEventCount(type)
- **Args:**
  - `type` *(string)* — Event type
- **Returns:**
  - `value` *(number)* — Number of event available
```lua
local count = GetEventCount("matchended")
for i=1, count do
	local name1, name2, score1, score2 = GetEvent("matchended", i)
end
```

### [Event] PostEvent(eventName, [param1, param2, .., paramN])
- **Args:**
  - `eventName` *(string)* — Event name
  - `param1, param2, .., paramN` *(any, optional)* — Optional parameters to send with the event.
```lua
PostEvent("matchended", "team1", "team2", 5, 10)
```

### [Event] returnValues = GetEvent(type, index)
- **Args:**
  - `type` *(string)* — Event type
  - `index` *(number)* — Event index (starting with one)
- **Returns:**
  - `returnValues` *(varying)* — Return values depending on event type
```lua
local count = GetEventCount("matchended")
for i=1, count do
	local name1, name2, score1, score2 = GetEvent("matchended", i)
end
```

---
**Navigation:** [_INDEX](_INDEX.md)