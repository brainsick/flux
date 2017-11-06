# flux
A fast, lightweight tweening library for Tabletop Simulator.

## Installation
The [flux.ttslua](flux.ttslua?raw=1) file should be dropped into your Tabletop
Simulator [include directory](http://blog.onelivesleft.com/2017/08/atom-tabletop-simulator-package.html).

## Initialization

```lua
#include flux

local fluxUpdateTime = os.time()
function onUpdate ()
  local now = os.time()
  local deltaTime = now - fluxUpdateTime
  flux.update(deltaTime)
  fluxUpdateTime = now
end
```

## Example Usage

```lua
#include flux

local positionTweens = {}

--[[ The onUpdate event is called once per frame. --]]
local fluxUpdateTime = os.time()
function onUpdate ()
  -- advance all flux tweens
  local now = os.time()
  local deltaTime = now - fluxUpdateTime
  flux.update(deltaTime)
  fluxUpdateTime = now

  -- flux only tweens the numbers, here we apply them
  -- setPositionSmooth is a shortcut to avoiding collisions while moving
  for k, v in pairs(positionTweens) do
    k.setPositionSmooth({ x = v.x, y = v.y, z = v.z })
  end
end

function onObjectDropped(player_color, dropped_object)
  if player_color == 'Black' then

    -- establish where we're starting
    local objPos = dropped_object.getPosition()

    -- define where we're going
    local randx1 = math.random(55) - (55 / 2.0)
    local randz1 = math.random(35) - (35 / 2.0)

    local randx2 = math.random(55) - (55 / 2.0)
    local randz2 = math.random(35) - (35 / 2.0)

    -- initialize the tween
    positionTweens[dropped_object] = { x = objPos.x, y = objPos.y, z = objPos.z }
    local positionTween = flux.to(positionTweens[dropped_object], 1, { x = randx1, y = 2, z = randz1 })
      :after(positionTweens[dropped_object], 1, { x = randx2, y = 2, z = randz2 }):delay(1)
      :oncomplete(function() positionTweens[dropped_object] = nil end)
  end
end
```

## More Information

See the [parent project](https://github.com/rxi/flux) for more thorough documentation.

## License
This library is free software; you can redistribute it and/or modify it under
the terms of the MIT license. See [LICENSE](LICENSE) for details.
