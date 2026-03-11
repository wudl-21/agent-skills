# Sprite

Sprites are 2D images in PNG or JPG format that can be drawn into the world. Sprites can be
drawn with ot without depth test (occluded by geometry). Sprites will not be affected by lighting
but they will go through post processing. If you want to display positioned information to the player as
an overlay, you probably want to use the Ui functions in combination with UiWorldToPixel instead.

### [API] handle = LoadSprite(path)
- **Args:**
  - `path` *(string)* — Path to sprite. Must be PNG or JPG format.
- **Returns:**
  - `handle` *(number)* — Sprite handle
```lua
function client.init()
	arrow = LoadSprite("gfx/arrowdown.png")
end
```

### [API] DrawSprite(handle, transform, width, height, [r], [g], [b], [a], [depthTest], [additive], [fogAffected])
- **Args:**
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
```lua
function client.init()
	arrow = LoadSprite("gfx/arrowdown.png")
end

function client.tick()
	--Draw sprite using transform
	--Size is two meters in width and height
	--Color is white, fully opaue
	local t = Transform(Vec(0, 10, 0), QuatEuler(0, GetTime(), 0))
	DrawSprite(arrow, t, 2, 2, 1, 1, 1, 1)
end
```

---
**Navigation:** [_INDEX](_INDEX.md)