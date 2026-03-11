# Module: eventlog

Displays a scrolling list of game events (e.g. kills) in the top-right corner.  
Messages are **posted server-side** and broadcast to all players. `playerdied` events are posted automatically via `eventlogTick`.

**Navigation:** [_INDEX](_INDEX.md) | [countdown](countdown.md) | [hud](hud.md) | [spawn](spawn.md) | [spectate](spectate.md) | [stats](stats.md) | [teams](teams.md) | [tools](tools.md) | [ui](ui.md) | [util](util.md)

---

## Usage Pattern

```lua
function server.tick(dt)
    eventlogTick(dt)
    if somethingNoteworthy then
        eventlogPostMessage({playerId, "did something cool!"}, 5.0)
        somethingNoteworthy = false
    end
end

function client.draw(dt)
    eventlogDraw(dt)
end
```

---

## Functions

### [API] eventlogPostMessage(items, time)
Post a message to the event log (server). Broadcasts to all players.

| Param | Type | Description |
|---|---|---|
| `items` | table | List of item descriptors (see below) |
| `time` | number | Display duration in seconds |

**Item descriptor fields:**
```
text        (string)  — Text to display
textColor   ({r,g,b}) — Text color
color       ({r,g,b}) — Background color
icon        (string)  — Icon image path
iconTint    ({r,g,b}) — Icon color tint
playerId    (number)  — Auto-sets color/icon; highlights if local player
iconRight   (bool)    — Move icon to the right side of text
```

**Auto-conversion shortcuts:**
- A plain `string` → `{text = x}`
- A plain `number` → `{playerId = x}`

---

### [API] eventlogTick(dt)
Update event log (server). Detects and posts `playerdied` events automatically.

| Param | Type | Description |
|---|---|---|
| `dt` | number | Time step in seconds |

---

### [API] eventlogDraw(dt[, playerColors])
Draw event log on screen (client). Decrements display timers and fades old messages.

| Param | Type | Description |
|---|---|---|
| `dt` | number | Delta time (decrements message display timer) |
| `playerColors` | table | *(optional)* Map of `playerId → {r,g,b}` |
