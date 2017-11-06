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
-- advance all flux tweens
  local now = os.time()
  local deltaTime = now - fluxUpdateTime
  flux.update(deltaTime)
  fluxUpdateTime = now
end
```

## Example Usage

```lua
local positionTweens = {}

-- when the Black player drops an object move it to a random location, pause,
-- and move it to another random location, because reasons
function onObjectDropped(player_color, dropped_object)
  if player_color == 'Black' then

    -- establish where we're starting
    local objPos = dropped_object.getPosition()
    positionTweens[dropped_object] = { x = objPos.x, y = objPos.y, z = objPos.z }

    -- define the first random location
    local firstRandomLocation = {
      x = math.random(55) - (55 / 2.0),
      y = 2,
      z = math.random(35) - (35 / 2.0)
    }

    -- define the second random location
    local secondRandomLocation = {
      x = math.random(55) - (55 / 2.0),
      y = 2,
      z = math.random(35) - (35 / 2.0)
    }

    -- define the tween
    local positionTween = flux
      -- tween to the first random location in 1 second
      .to(positionTweens[dropped_object], 1, firstRandomLocation)
      -- apply the tween to our objects
      :onupdate(function() dropped_object.setPositionSmooth(positionTweens[dropped_object]) end)
      -- tween to the second random location in 1 second, after a delay of 1 second
      :after(positionTweens[dropped_object], 1, secondRandomLocation):delay(1)
      -- apply the tween to our objects
      :onupdate(function() dropped_object.setPositionSmooth(positionTweens[dropped_object]) end)
      -- clean up tween data structure
      :oncomplete(function() positionTweens[dropped_object] = nil end)
  end
end
```

## More Information

See the [parent project](https://github.com/rxi/flux) for more thorough documentation.

## License
This library is free software; you can redistribute it and/or modify it under
the terms of the MIT license. See [LICENSE](LICENSE) for details.
