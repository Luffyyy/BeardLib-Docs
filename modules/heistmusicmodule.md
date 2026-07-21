# Heist Music

Updated for version 4.4.

## Module Definition

The module is inherited from [ItemModuleBase](https://luffyyy.gitbook.io/beardlib/modules/modulebase#itemmodulebase). So base parameters can be found there.

This modules creates a heist track.

### Module name

The name of the module you use as the meta of the module definition is 'HeistMusic' or 'HeistMusicModule' if `_force_search` is set to true in the module definition.

### XML Structure

```markup
<HeistMusic id directory>
    <event name source/>
</HeistMusic>
```

#### `<HeistMusic ...>`

| Parameter | Type   | Description                                |
| --------- | ------ | ------------------------------------------ |
| id        | String | The ID of the track. This has to be unique |
| volume    | Float  | The volume of the music (from 0 to 1)      |

##### `<event name source>`

| Parameter                       | Type    | Description                                                                                                                   |
| ------------------------------- | ------- | ----------------------------------------------------------------------------------------------------------------------------- |
| name                            | String  | The name of the event. At the moment it can only be `setup` for stealth, `anticipation` for build up, `assault` and `control` |
| source                          | String  | The path to the .ogg file that the track should play in the event                                                             |
| start\_source                   | String  | Optional path to sound to play before playing `source`                                                                        |
| volume                          | Float   | Volume of this specific music event                                                                                           |
| allow\_switch                   | Bool    | Whether a new track is selected after every loop (default) or only once per event if multiple `<track>` nodes are available   |
| weight                          | Integer | Base weight of any tracks added via `<track>` nodes                                                                           |
| play_order                      | String  | Order in which multiple tracks are played, possible values are `random` (default), `shuffle`, `sequence` and `loop`           |

###### `<track source>`

`<track>` nodes can be placed as children of `<event>` nodes to specify multiple tracks for a single event.\
They inherit any values set on the `<event>` node if not specified in the `<track>` node itself.

| Parameter     | Type    | Description                                                       |
| ------------- | ------- | ----------------------------------------------------------------- |
| source        | String  | The path to the .ogg file that the track should play in the event |
| start\_source | String  | Optional path to sound to play before playing `source`            |
| volume        | Float   | Volume of this specific music track                               |
| weight        | Integer | Weight of this track for being chosen randomly                    |

Tracks will be selected according to the play_order set on the parent `<event>` node.  
When `allow_switch` is true (the default if not specified), track selection happens whenever a track is done playing a single loop, when it's false, track selection only happens once per event.  

| Mode          | Description                                                                                                    |
| ------------- | -------------------------------------------------------------------------------------------------------------- |
| random        | Selects a random track according to their weight each time                                                     |
| shuffle       | Selects a random track according to their weight each time but avoids duplicates until every track played once |
| sequence      | Selects tracks in the order they are defined and keeps playing the last one when it reaches the end            |
| loop          | Selects tracks in the order they are defined and starts again from the beginning when it reaches the end       |


### Example

This example is what you would put inside your main node within your [mod config](https://github.com/GreatBigBushyBeard/PAYDAY-2-BeardLib/wiki/Module-Config)

```markup
<HeistMusic id="my_heist_music_name" directory="sounds">
    <event name="setup" source="stealth.ogg"/>
    <event name="anticipation" source="buildup.ogg"/>
    <event name="assault" source="assault.ogg"/>
    <event name="control" source="control.ogg"/>
</HeistMusic>
```

This will add the track. You will still need to localize the name of the track. In this case menu\_jukebox\_my\_heist\_music\_name and menu\_jukebox\_screen\_my\_heist\_music\_name will need to be localized.

Having multiple tracks for a single event will allow you to create a random track selection, similar to how "Ode to Greed" or "Break the Rules" have a chance to play a lyric version of their assault tracks.\
For this you would simply set up an event like this

```markup
<event name="assault">
    <track source="assault.ogg" weight="10"/>
    <track source="assault_lyrics.ogg" weight="1"/>
</event>
```

The weight defines how likely a track is going to be selected whenever this event plays. In the example above this would mean the regular assault track is 10 times more likely to play than the lyric version.

### Template

Download template here: [https://modworkshop.net/mydownloads.php?action=view\_down\&did=21651](https://modworkshop.net/mydownloads.php?action=view\_down\&did=21651)

### Functions

#### Get

| Function                  | Return Type  | Description                                       |
| ------------------------- | ------------ | ------------------------------------------------- |
| MakeBuffer(String source) | XAudioBuffer | Called by RegisterHook to create an XAudio buffer |
