# flux
A fast, lightweight tweening library for Tabletop Simulator.

## Installation
The [flux.ttslua](flux.ttslua?raw=1) file should be dropped into your Tabletop
Simulator [include directory](http://blog.onelivesleft.com/2017/08/atom-tabletop-simulator-package.html).

## Usage

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
## More Information

See the [parent project](https://github.com/rxi/flux) for more thorough documentation.

## License
This library is free software; you can redistribute it and/or modify it under
the terms of the MIT license. See [LICENSE](LICENSE) for details.
