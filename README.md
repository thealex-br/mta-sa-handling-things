## Main Functions
```lua
function guessHex(byte, value)
    return "0x"..string.reverse(string.format("%0"..byte.."x", value))
end

function getVehicleHandlingFlags(vehicle, flags, byte, value)
    local hnd = getVehicleHandling(vehicle)[flags]
    local hex = guessHex(byte, value)
    return bitAnd(hnd, hex) ~= 0
end

function setVehicleHandlingFlags(vehicle, flags, byte, value, bool)
    local hnd = getVehicleHandling(vehicle)[flags]
    local hex = guessHex(byte, value)
    return setVehicleHandling(vehicle, flags, bool and bitOr(hnd, hex) or bitAnd(hnd, bitNot(hex)))
end
```

## Examples of use
### Wider Wheels
```lua
setVehicleHandlingFlags(vehicle, "handlingFlags", 3, 8, true)
setVehicleHandlingFlags(vehicle, "handlingFlags", 4, 8, true)
```
### Vehicles won't exceed maxVelocity
```lua
setVehicleHandlingFlags(vehicle, "handlingFlags", 7, 1, true)
```
### Get the real maxVelocity
```lua
function getVehicleMaxVelocity(vehicle)
    local maxSpeed = getVehicleHandling(vehicle).maxVelocity
    return getVehicleHandlingFlags(vehicle, "handlingFlags", 7, 1) and maxSpeed or (maxSpeed + 0.2 * maxSpeed)
end
```

see [Project Cerbera](https://projectcerbera.com/gta/sa/tutorials/handling) to known more about handlingFlags
