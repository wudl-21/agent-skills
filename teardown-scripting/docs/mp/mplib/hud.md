# Module: hud

Multiplayer HUD utilities. Handles damage feedback, timers, round indicators, scoreboards, world markers, banners, and game setup UI.

- **Server-side:** initialization and game state control (synced via `shared._hud`)
- **Client-side:** rendering all HUD elements and reacting to events

**Navigation:** [_INDEX](_INDEX.md) | [countdown](countdown.md) | [eventlog](eventlog.md) | [spawn](spawn.md) | [spectate](spectate.md) | [stats](stats.md) | [teams](teams.md) | [tools](tools.md) | [ui](ui.md) | [util](util.md)

---

## Functions

### [API] hudAddUnstuckButton()
*(server)* Adds an "Unstuck" button to the pause menu. Allows clients to respawn themselves if stuck. 10-second cooldown per client.

---

### [API] hudTick(dt)
*(client)* Process `playerhurt` events for damage indicators, update per-player health bar state, and inject Unstuck button into pause menu if enabled.

| Param | Type | Description |
|---|---|---|
| `dt` | number | Delta time in seconds |

---

### [API] hudDrawTimer(time[, alpha])
*(client)* Draw countdown timer in `MM:SS` format near top of screen. Plays warning sound during last seconds.

| Param | Type | Description |
|---|---|---|
| `time` | number | Time in seconds |
| `alpha` | number | *(optional)* Alpha multiplier [0..1] |

---

### [API] hudShowBanner(text, color)
*(client)* Enqueue an animated banner. Consumed by `hudDrawBanner`.

| Param | Type | Description |
|---|---|---|
| `text` | string | Banner text |
| `color` | table | Background color `{r,g,b,a}` |

---

### [API] hudDrawScoreboard(show, title, columns, groups)
*(client)* Render animated in-game scoreboard.

| Param | Type | Description |
|---|---|---|
| `show` | bool | Whether scoreboard is visible |
| `title` | string | Title at top (nil/empty = no title) |
| `columns` | table | List of `{name, width, align?}` column descriptors |
| `groups` | table | List of group tables (see below) |

**Group table structure:**
```
name    (string)   — Group header text
color   ({r,g,b})  — Group accent color
outline (bool)     — Draw outline (optional)
dim     (bool)     — De-emphasize style (optional)
rows    (table)    — List of {player=id, columns={...values...}}
```

---

### [API] hudDrawResults(bannerLabel, bannerColor, title, columns, groups[, continueFunction[, continueLabel]])
*(client)* Render end-of-match results scoreboard with animated banner and continue button.

| Param | Type | Description |
|---|---|---|
| `bannerLabel` | string | Banner text |
| `bannerColor` | table | Banner color `{r,g,b,a}` |
| `title` | string | Scoreboard title |
| `columns` | table | Column descriptors (same as `hudDrawScoreboard`) |
| `groups` | table | Group tables (same as `hudDrawScoreboard`) |
| `continueFunction` | function | *(optional)* Callback on "Play Again" press |
| `continueLabel` | string | *(optional)* Button label (default: "Play Again") |

**Returns:** `boardWidth`, `boardHeight`, `animParam` (0..1)

---

### [API] hudDrawScore2Teams(team1Color, team1Score, team2Color, team2Score[, alpha])
*(client)* Draw two team scores side-by-side in colored boxes at top of screen.

| Param | Type | Description |
|---|---|---|
| `team1Color` | `{r,g,b}` | Team 1 color |
| `team1Score` | number | Team 1 score |
| `team2Color` | `{r,g,b}` | Team 2 color |
| `team2Score` | number | Team 2 score |
| `alpha` | number | *(optional)* Alpha [0..1] |

---

### [API] hudDrawRounds(currentRound, maxRound[, width])
*(client)* Show round indicator (e.g. "Round 2/5") below main timer.

| Param | Type | Description |
|---|---|---|
| `currentRound` | number | Current round (1-based) |
| `maxRound` | number | Total rounds |
| `width` | number | *(optional)* Min pixel width |

---

### [API] hudDrawRoundScroreBreakdown(header, teamNames, teamColors, scoreTable, drawTotal[, highlightColumn[, minWidth]])
*(client)* Draw per-round score table with team rows.

| Param | Type | Description |
|---|---|---|
| `header` | string | Table title |
| `teamNames` | table | `{string, ...}` team names |
| `teamColors` | table | `{{r,g,b}, ...}` team colors |
| `scoreTable` | table | `scoreTable[round][teamIndex] = score` |
| `drawTotal` | bool | Show total column |
| `highlightColumn` | number | *(optional)* Round index to highlight (≤0 disables) |
| `minWidth` | number | *(optional)* Min table width |

**Returns:** `totalWidth`, `totalHeight` (pixels)

```lua
-- Example
local roundScores = {}
roundScores[1] = { team1_round1_score, team2_round1_score }
roundScores[2] = { team1_round2_score, team2_round2_score }
hudDrawRoundScroreBreakdown("Current score", {"Red team","Blue team"}, {{1,0,0},{0,0,1}}, roundScores, false, currRound)
```

---

### [API] hudDrawTitle(dt, title[, show])
*(client)* Fade in/out a title banner near top of screen. Auto-hides after 5s if `show` is nil.

| Param | Type | Description |
|---|---|---|
| `dt` | number | Time step |
| `title` | string | Title text |
| `show` | bool | *(optional)* Explicit visibility; nil = auto-hide after 5s |

---

### [API] hudDrawInformationMessage(message, alpha)
*(client)* Draw a centered small text panel near top of screen.

| Param | Type | Description |
|---|---|---|
| `message` | string | Text |
| `alpha` | number | Alpha [0..1] |

---

### [API] hudDrawCountDown(time)
*(client)* Draw large numeric countdown in screen center with fade. Values ≤ 0 disable rendering.

| Param | Type | Description |
|---|---|---|
| `time` | number | Remaining time in seconds |

---

### [API] hudDrawRespawnTimer(time)
*(client)* Draw "Respawn in..." message and numeric countdown when dead. Triggers brief fade near respawn.

| Param | Type | Description |
|---|---|---|
| `time` | number | Remaining respawn time (nil or ≤0 = alive) |

---

### [API] hudDrawPlayerWorldMarkers(players, lineOfSightRequired, maxRange[, color])
*(client)* Build and render world markers for each valid remote player via `hudDrawWorldMarkers`.

| Param | Type | Description |
|---|---|---|
| `players` | table | List of player IDs |
| `lineOfSightRequired` | bool | Hide when occluded |
| `maxRange` | number | Max display range in meters |
| `color` | table | *(optional)* `{r,g,b,a}`, default white |

---

### [API] hudDrawWorldMarkers(markers)
*(client)* Draw arbitrary in-world markers projected to screen space.

| Param | Type | Description |
|---|---|---|
| `markers` | table | List of marker tables (see below) |

**Marker fields:**
```
pos                (Vec3)       — World-space position
offset             (Vec3)       — Offset added before projection (optional)
color              ({r,g,b,a})  — Marker color
label              (string)     — Text label (optional)
maxRange           (number)     — Max distance, default 9999 (optional)
lineOfSightRequired(bool)       — Hide when occluded
player             (number)     — Player ID for occlusion/vehicle checks (optional)
icon               (string)     — Icon image path (optional)
uiOffset           ({x,y})      — 2D offset after projection (optional)
drawIconInView     (bool)       — Draw icon when on-screen (optional)
iconColor          ({r,g,b})    — Icon color override (optional)
```

```lua
-- Example
local worldMarkers = {}
for p in Players() do
    worldMarkers[#worldMarkers+1] = {
        pos = GetPlayerTransform(p).pos,
        color = {1,1,1},
        label = GetPlayerName(p),
        offset = Vec(0,2,0),
        lineOfSightRequired = false,
        player = p
    }
end
hudDrawWorldMarkers(worldMarkers)
```

---

### [API] hudDrawDamageIndicators(dt)
*(client)* Draw directional fade-out indicators of recent damage sources.

| Param | Type | Description |
|---|---|---|
| `dt` | number | Delta time to fade indicators |

---

### [API] hudGameIsSetup()
*(client/server)* Check if host pressed Start in game setup UI.

**Returns:** `bool` — `true` if setup complete.

---

### [API] hudDrawGameSetup(settings)
*(client)* Draw host-only game setup UI.
- **Host:** Shows Start + Settings buttons. Settings panel lets host choose options.
- **Clients:** Shows "Waiting for host..." until host starts.

| Param | Type | Description |
|---|---|---|
| `settings` | table | Array of config groups (see below) |

**Returns:** `bool` — `true` if Play/Start was pressed.

**Settings table format:**
```lua
local settings = {
  {
    title = "",
    items = {
      {
        key     = "savegame.mod.settings.time",
        label   = "Time",
        info    = "Select match time.",
        options = {
          { label = "05:00", value = 5*60 },
          { label = "10:00", value = 10*60 },
        }
      },
      {
        key     = "savegame.mod.settings.unlimited",
        label   = "Unlimited tool ammo",
        info    = "Toggle unlimited ammo",
        options = {
          { label = "On",  value = 1 },  -- 1/0 for booleans
          { label = "Off", value = 0 },
        }
      }
    }
  }
}
```

---

### [API] hudDrawPlayerList()
*(client)* Show panel listing all session players, with local player highlighted.

---

### [API] hudDrawGameModeHelpText(header, text[, headerColor])
*(client)* Draw a help/rules text box for the current game mode.

| Param | Type | Description |
|---|---|---|
| `header` | string | Header text (nil/"" = no header) |
| `text` | string | Body text |
| `headerColor` | table | *(optional)* `{r,g,b}` header color |

---

### [API] hudDrawResultsAnimation(time, text[, backgroundColor])
*(client)* Draw animated end-of-match banner with camera motion and intro/outro sounds.

| Param | Type | Description |
|---|---|---|
| `time` | number | Elapsed animation time in seconds |
| `text` | string | Banner text |
| `backgroundColor` | table | *(optional)* `{r,g,b,a}`, default `COLOR_BLACK_TRNSP` |

**Returns:** `bool` — `true` when animation fully finished.

---

### [API] hudDrawFade(dt)
*(client)* Draw full-screen fade effect from queued `hudFade` events (fade-in, hold, fade-out). Optionally disables HUD while fully black.

| Param | Type | Description |
|---|---|---|
| `dt` | number | Delta time in seconds |

---

### [API] hudDrawBanner(dt)
*(client)* Consume and animate banners enqueued via `hudShowBanner`. Call continuously.

| Param | Type | Description |
|---|---|---|
| `dt` | number | Delta time to advance banner animation |
