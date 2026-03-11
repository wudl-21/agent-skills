# Teardown SP API — Sprite
> **Version:** 1.7.0 | **Mode:** Singleplayer | **Language:** Lua 5.1

Sprites are 2D images in PNG or JPG format that can be drawn into the world. Sprites can be drawn with ot without depth test (occluded by geometry). Sprites will not be affected by lighting but they will go through post processing. If you want to display positioned information to the player as an overlay, you probably want to use the Ui functions in combination with UiWorldToPixel instead.

---

### [API] LoadSprite

```lua
handle = LoadSprite(path)
```

**Arguments:**

- `path` *(string)* — Path to sprite. Must be PNG or JPG format.

**Example:**

```lua
function init()
	arrow = LoadSprite("gfx/arrowdown.png")
end
```

---

### [API] DrawSprite

```lua
DrawSprite(handle, transform, width, height, [r], [g], [b], [a], [depthTest], [additive], [fogAffected])
```

Draw sprite in world at next frame. Call this function from the tick callback.

**Arguments:**

- `handle` *(number)* — Sprite handle

- `transform` *(TTransform)* — Transform

- `width` *(number)* — Width in meters

- `height` *(number)* — Height in meters

- `r` *(number, optional)* — Red color. Default 1.0.

- `g` *(number, optional)* — Green color. Default 1.0.

- `b` *(number, optional)* — Blue color. Default 1.0.

- `a` *(number, optional)* — Alpha. Default 1.0.

- `depthTest` *(boolean, optional)* — Depth test enabled. Default false.

- `additive` *(boolean, optional)* — Additive blending enabled. Default false.

- `fogAffected` *(boolean, optional)* — Enable distance fog effect. Default false.

**Example:**

```lua
function init()
	arrow = LoadSprite("gfx/arrowdown.png")
end

function tick()
	--Draw sprite using transform
	--Size is two meters in width and height
	--Color is white, fully opaue
	local t = Transform(Vec(0, 10, 0), QuatEuler(0, GetTime(), 0))
	DrawSprite(arrow, t, 2, 2, 1, 1, 1, 1)
end
```

---


---
**Navigation:**
[Teardown scripting](00_overview.md) | [Parameters](01_parameters.md) | [Script control](02_script_control.md) | [Registry](03_registry.md) | [Vector math](04_vector_math.md) | [Entity](05_entity.md) | [Body](06_body.md) | [Shape](07_shape.md) | [Location](08_location.md) | [Joint](09_joint.md) | [Animation](10_animation.md) | [Light](11_light.md) | [Trigger](12_trigger.md) | [Screen](13_screen.md) | [Vehicle](14_vehicle.md) | [Player](15_player.md) | [Sound](16_sound.md) | **Sprite** | [Scene queries](18_scene_queries.md) | [Particles](19_particles.md) | [Spawning](20_spawning.md) | [Miscellaneous](21_miscellaneous.md) | [User Interface](22_ui.md)