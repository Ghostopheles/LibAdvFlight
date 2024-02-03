# LibAdvFlight

A simple library to track events related to the advanced flying system. Requires [LibStub](https://www.curseforge.com/wow/addons/libstub) (included in packaged releases).

## Usage

Add LibAdvFlight-1.0 to your addon by either specifying it in your `.pkgmeta` file, or by downloading a packaged version from this repo, and loading the `lib.xml` file.
```lua
local LibAdvFlight = LibStub:GetLibrary("LibAdvFlight-1.0");
local Events = LibAdvFlight.Events;

local function OnAdvFlyStart()
    print("Player has taken off!");
end

LibAdvFlight.RegisterCallback(Events.ADV_FLYING_START, OnAdvFlyStart);
```

## API

### Events
All event names can be found in `LibAdvFlight.Events`. Event args will be listed beneath any events that include them.

##### Advanced Flying Enable State
Whether or not the player is *able* to advanced fly.
- `ADV_FLYING_ENABLED`: Advanced flying has been enabled. This fires when the player mounts up on an advanced flyable mount, or when using Soar as a Dracthyr.
- `ADV_FLYING_DISABLED`: Advanced flying has been disabled. Occurs when the player dismounts, or if the zone is not advanced flyable.
- `ADV_FLYING_ENABLE_STATE_CHANGED`: Advanced flying enable state has changed. This acts as a 'toggle', useful if you would rather register one event instead of both `ADV_FLYING_{ENABLED | DISABLED}`.
    - `advFlyingEnabled` - `boolean`: Indicates the new advanced flying enable state.

##### Advanced Flying State
Whether the player is actively using advanced flight or not (flying or grounded, essentially).
- `ADV_FLYING_START`: Player has taken off and begun flying using advanced flight.
- `ADV_FLYING_END`: Player has either landed or dismounted and is no longer flying.
- `ADV_FLYING_STATE_CHANGED`: Advanced flying state has changed, as with `ADV_FLYING_ENABLE_STATE_CHANGED`, this is acts as a 'toggle' event.
    - `isAdvFlying` - `boolean`: Indicates the new 'flying' state of the player.

##### Vigor State
Player's current vigor/energy state
- `VIGOR_MAX_CHANGED`: Player's maximum vigor value has changed. This event may fire multiple times upon first mounting up due to Blizzard's nonsense.
    - `vigorMax` - `number`: Player's maximum vigor value.
- `VIGOR_CHANGED`: Player's current vigor value has changed. This event may also have unexpected behavior upon first mounting up (like values above 6??).
    - `vigorCurrent` - `number`: Player's current vigor value.

### Functions
Events are great and all, but useless without a way to listen for them.

##### `LibAdvFlight.RegisterCallback(event, callback, owner?)`
Registers a callback for the given event.
- `event` - `string`: Event name. Names can be found in `LibAdvFlight.Events`.
- `callback` - `function`: Callback function called when the given event is fired and will be passed the event's arguments.
- `owner?` - `table`: Object passed to callback as the first argument to the `callback` function, same as the base CallbackRegistry, though if it's not provided, passes just the event arguments.

Returns a callback handle - a table containing an `Unregister` function to unregister the callback.

##### `LibAdvFlight.IsAdvFlyEnabled()`
Returns whether or not advanced flying is currently enabled. See the **Events** section above for more details.

##### `LibAdvFlight.IsAdvFlying()`
Returns whether or not the player is currently flying using advanced flight. See the **Events** section above for more details.

##### `LibAdvFlight.GetMaxVigor()`
Returns the player's maximum vigor level. Returns `0` if not mounted on an advanced flight mount.

##### `LibAdvFlight.GetCurrentVigor()`
Returns the player's current vigor level. Returns `0` if not mounted on an advanced flight mount.

##### `LibAdvFlight.GetForwardSpeed()`
Returns the current `forwardSpeed` of the player. Returns `0` if not mounted on an advanced flight mount.

##### `LibAdvFlight.GetForwardSpeedRounded()`
Returns the current `forwardSpeed`, rounded to the nearest whole number. Returns `0` if not mounted on an advanced flight mount.

##### `LibAdvFlight.GetHeading()`
Returns the player's current heading (direction) in degrees. Returns `nil` inside instances.

##### `LibAdvFlight.GetHeadingRounded()`
Returns the player's current heading in degrees, rounded to the nearest whole number. Returns `nil` inside instances.

##### `LibAdvFlight.GetHeadingRadians()`
Returns the player's current heading in radians. Returns `nil` inside instances.
