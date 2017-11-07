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
-- when the Black player drops an object move it to a random position, pause,
-- and move it to another random position, because reasons
function onObjectDropped(player_color, dropped_object)
  if player_color == 'Black' then

    -- establish the starting position
    local objPos = dropped_object.getPosition()
    local tweenPosition = {
      x = objPos.x,
      y = objPos.y,
      z = objPos.z
    }

    local function createRandomPosition()
      local tablewidth, tableheight = 55, 35
      local offsetx, offsetz = tablewidth / 2.0, tableheight / 2.0
      return {
        x = math.random(tablewidth) - offsetx,
        y = 2,
        z = math.random(tableheight) - offsetz
      }
    end

    local function applyTweenPosition()
      dropped_object.setPositionSmooth(tweenPosition)
    end

    -- define the tween
    local positionTween = flux
      -- tween to the first random position in 1 second
      .to(tweenPosition, 1, createRandomPosition())
      -- apply the tween to our object
      :onupdate(applyTweenPosition)
      -- tween to the second random position in 1 second, after a delay of 1 second
      :after(tweenPosition, 1, createRandomPosition()):delay(1)
      -- apply the tween to our object
      :onupdate(applyTweenPosition)
  end
end
```

![Resulting Tween](https://user-images.githubusercontent.com/1111573/32465298-a5682c70-c308-11e7-978f-05457931806d.gif)


## More Information

See the [parent project](https://github.com/rxi/flux) for more thorough documentation.

## License
This library is free software; you can redistribute it and/or modify it under
the terms of the MIT license. See [LICENSE](LICENSE) for details.
