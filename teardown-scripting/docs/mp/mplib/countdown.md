# Module: countdown

Displays a countdown timer and locks players during the countdown period.  
Server initializes and ticks; client draws. Audio plays automatically.

**Navigation:** [_INDEX](_INDEX.md) | [eventlog](eventlog.md) | [hud](hud.md) | [spawn](spawn.md) | [spectate](spectate.md) | [stats](stats.md) | [teams](teams.md) | [tools](tools.md) | [ui](ui.md) | [util](util.md)

---

## Usage Pattern

```lua
function server.init()
    countdownInit(3.0)
end

function server.tick(dt)
    if countdownTick(dt) then
        return  -- Match hasn't started yet
    end
    -- countdown is done, do game logic
end

function client.draw()
    countdownDraw()
end
```

---

## Functions

### [API] countdownInit(countdownSeconds)
Initialize countdown timer. Call once on **server** during setup.

| Param | Type | Description |
|---|---|---|
| `countdownSeconds` | number | Duration of countdown in seconds |

---

### [API] countdownTick(dt)
Decrement timer by `dt`. Players are **locked** during countdown. Call each **server** tick.

| Param | Type | Description |
|---|---|---|
| `dt` | number | Time step in seconds |

**Returns:** `bool` — `true` if timer is still active.

---

### [API] countdownDraw()
Draw countdown timer and "Match starts in..." label. Call each **client** draw frame.

**Returns:** `bool` — `true` if timer is still active.

---

### [API] countdownDone()
Check if countdown has completed.

**Returns:** `bool` — `true` if countdown reached 0.

---

### [API] countdownGetTime()
Get remaining time (clamped to 0).

**Returns:** `number` — Remaining time in seconds.
