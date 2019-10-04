# forgottenserver with support for OTClientV8 

### OTClientV8: https://github.com/OTCv8/otclientv8

## Supported features:
### New walking
### New ping
### Extended map view (optional)
### Ingame shop system

## How to enable this features in OTClientV8?
You must enable following features:
- 25 - GameExtendedClientPing
- 30 - GameChangeMapAwareRange
- 80 - GameExtendedOpcode
- 90 - GameNewWalking

There are 3 ways to do it:
1. Add it in login.php
```
$features = array(
    25 => true, // GameExtendedClientPing
    30 => true, // GameChangeMapAwareRange
    80 => true, // GameExtendedOpcode
    90 => true, // GameNewWalking
);
```

2. Add it in `data/game_features/features.lua`
```
function updateFeatures(version)
    g_game.resetFeatures()

    g_game.enableFeature(GameExtendedClientPing)
    g_game.enableFeature(GameChangeMapAwareRange)
    g_game.enableFeature(GameExtendedOpcode)
    g_game.enableFeature(GameNewWalking)
    
    ...
```

3. Add it in `Servers` in `init.lua` after `ip:port:version` like this:
```
Servers = {
  OTClientV8 = "otclient.ovh:7171:1099:25:30:80:90"
}
```

## Map aware range
![OTClientV8 with extended view range](view.jpg?raw=true "OTClientV8 with extended view range")

If you want to change how many tiles can be seen in OTClientV8, just edit map.h:
```
static constexpr int32_t maxViewportX = 11 + 4; //min value: maxClientViewportX + 1
static constexpr int32_t maxViewportY = 11 + 4; //min value: maxClientViewportY + 1
static constexpr int32_t maxClientViewportX = 8 + 4;
static constexpr int32_t maxClientViewportY = 6 + 4;
```

Set `maxClientViewportX` and `maxClientViewportY` to whatever value you like. Just remember to keep `maxClientViewportX` and `maxClientViewportY` bigger than `maxClientViewportX` and `maxClientViewportY`.

## New walking
The main difference between old walking and new walking is that new walking allow player to make 1 more step without server confirmation (prewalk). Because of that, even with high ping, walking is smooth. For example, on version without new walking if it takes 100 ms to make a step, only players with ping lower than 100 ms will have smooth walking. With new walking and 100 ms walk speed, player is able to walk twice without confirmations, so he won't feel lag while his ping is less than 200 ms. It is highly recommended to limit max speed to 100 ms per tile and to host server in North America. Then players from south america and europe will have ping less than 200 ms and will be able to walk smoothly almost all the time.

To better understand that, I made videos comparing walking in cipsoft client, otclient and otclientv8 when player have 50, 150 and 250 ms ping:

[![Cipsoft client](https://img.youtube.com/vi/HxT0jARByUs/0.jpg)](https://www.youtube.com/watch?v=HxT0jARByUs)
[![OTClient](https://img.youtube.com/vi/87b4OuNCgYU/0.jpg)](https://www.youtube.com/watch?v=87b4OuNCgYU)
[![OTClientV8](https://img.youtube.com/vi/05PAFF58Dzs/0.jpg)](https://www.youtube.com/watch?v=05PAFF58Dzs)


