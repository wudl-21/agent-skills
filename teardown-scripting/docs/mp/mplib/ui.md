# Module: ui

Standardized UI helper functions for rendering text, panels, buttons, and player visuals across mplib scripts. All functions are **client-side**.

**Navigation:** [_INDEX](_INDEX.md) | [countdown](countdown.md) | [eventlog](eventlog.md) | [hud](hud.md) | [spawn](spawn.md) | [spectate](spectate.md) | [stats](stats.md) | [teams](teams.md) | [tools](tools.md) | [util](util.md)

---

## Text

### [API] uiTextConstrained(text, font, fontSize, maxWidth[, maxLines])
Measure how much of a string fits within constraints. Uses binary search; appends ellipsis on overflow.

| Param | Type | Description |
|---|---|---|
| `text` | string | Text to measure |
| `font` | string | Font asset path |
| `fontSize` | number | Font size |
| `maxWidth` | number | Max allowed width |
| `maxLines` | number | *(optional)* Max lines; omit for single-line |

**Returns:** `fits (bool)`, `displayedText (string)` — original or truncated.

---

### [API] uiDrawTextConstrained(text, font, fontSize, maxWidth[, maxLines])
Draw constrained text. Renders overflow text in **red** to highlight layout issues.

| Param | Type | Same as `uiTextConstrained` |
|---|---|---|

---

### [API] uiDrawTextEllipsis(text, font, fontSize, maxWidth[, maxLines])
Draw constrained text with silent ellipsis truncation (no red highlight).

| Param | Type | Same as `uiTextConstrained` |
|---|---|---|

---

## Player Visuals

### [API] uiGetPlayerImage(playerId)
Get character preview image path for a player. Falls back to placeholder if none.

| Param | Type | Description |
|---|---|---|
| `playerId` | number | Player ID |

**Returns:** `string` — filepath.

---

### [API] uiDrawPlayerImage(playerId, width, height, roundingRadius[, outlineColor[, outlineThickness]])
Draw player avatar with optional rounded outline.

| Param | Type | Description |
|---|---|---|
| `playerId` | number | Player ID |
| `width` | number | Image width |
| `height` | number | Image height |
| `roundingRadius` | number | Corner radius |
| `outlineColor` | table | *(optional)* `{r,g,b,a}` |
| `outlineThickness` | number | *(optional)* |

---

### [API] uiDrawPlayerRow(playerId[, height], maxWidth[, color[, dim]])
Draw a full player row: avatar + name. Used in scoreboards and player lists.

| Param | Type | Description |
|---|---|---|
| `playerId` | number | Player ID |
| `height` | number | *(optional)* Row height (default: 32) |
| `maxWidth` | number | Max width for name text |
| `color` | table | *(optional)* Override color `{r,g,b}` |
| `dim` | boolean | *(optional)* Dim the row |

---

## Buttons

### [API] uiDrawPrimaryButton(title, width[, disabled])
Draw a styled primary action button.

| Param | Type | Description |
|---|---|---|
| `title` | string | Button text |
| `width` | number | Button width in pixels |
| `disabled` | boolean | *(optional)* Disable input |

**Returns:** `boolean` — `true` if clicked.

---

### [API] uiDrawSecondaryButton(title, width[, disabled])
Draw a styled secondary action button.

| Param | Type | Description |
|---|---|---|
| `title` | string | Button label |
| `width` | number | Button width |
| `disabled` | boolean | *(optional)* |

**Returns:** `boolean` — `true` if clicked.

---

### [API] uiDrawButton(title, width, color, hoverColor, outline[, disabled])
Draw a fully configurable button. Base implementation used by primary/secondary variants.

| Param | Type | Description |
|---|---|---|
| `title` | string | Button text |
| `width` | number | Button width |
| `color` | table | Background `{r,g,b,a}` |
| `hoverColor` | table | Hover color `{r,g,b,a}` |
| `outline` | boolean | Draw outline |
| `disabled` | boolean | *(optional)* |

**Returns:** `boolean` — `true` on click.

---

## Panels

### [API] uiDrawPanel(width, height[, radius])
Draw a translucent panel (used for dialogs, popups, player lists).

| Param | Type | Description |
|---|---|---|
| `width` | number | Panel width |
| `height` | number | Panel height |
| `radius` | number | *(optional)* Corner radius |

---

### [API] uiDrawTextPanel(message[, alpha])
Draw a text panel with background and padding (notifications, tooltips).

| Param | Type | Description |
|---|---|---|
| `message` | string | Text |
| `alpha` | number | *(optional)* Opacity multiplier |

---

### [API] uiDrawTextAndImagePanel(message, imageItem[, alpha])
Draw a panel with text and an icon image (objective info, etc.).

| Param | Type | Description |
|---|---|---|
| `message` | string | Text |
| `imageItem` | table | `{path=string, color={r,g,b}}` |
| `alpha` | number | *(optional)* Opacity multiplier |
