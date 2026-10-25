# Weapon Mod

Updated for version 3.38.

This page is missing some information.

## Module Definition

The module is inherited from [ItemModuleBase](https://luffyyy.gitbook.io/beardlib/modules/modulebase#itemmodulebase). So base parameters can be found there.

This module lets you add weapon parts.

### Module name

The name of the module you use as the meta of the module definition is 'WeaponMod' or 'WeaponModModule' if `_force_search` is set to true in the module definition.

### XML Structure

```markup
<WeaponMod id based_on type .../>
```

#### `<WeaponMod ...>`

| Parameter | Type | Description |
| :--- | :--- | :--- |
| id | String | The ID of the part |
| based\_on | String | An ID of an existing weapon part you want to base your part on. Full list [here](https://wiki.modworkshop.net/books/payday-2-mod-creation/page/weapon-mods) |
| type | String | The type of the part. Can be:  `ammo`, `barrel`, `barrel_ext`, `barrel_lock`, `bayonet`, `bipod`, `body`, `bolt`, `bonus`, `custom`, `cylinder`, `drag_handle`, `ejector`, `ejector_l`, `ejector_r`, `ejectorpin`, `extra`, `firepin`, `foregrip`, `foregrip_ext`, `gadget`, `grip`, `hammer`, `left_hammer`, `left_slug`, `lock_arm`, `lower_body`, `lower_receiver`, `lower_reciever`, `magazine`, `meter_l`, `meter_r`, `rail`, `right_hammer`, `right_slug`, `safety`, `sight`, `sight_special`, `slide`, `stock`, `stock_adapter`, `switch`, `swiwel_1`, `swiwel_2`, `underbarrel`, `upper_body`, `upper_receiver`, `upper_reciever`, `vertical_grip` |
| sub\_type | String | The sub type of the part. Like a silencer is a barrel extension \(barrel\_ext\) and a supressor. The types: `ammo`, `ammo_custom`, `ammo_dragons_breath`, `ammo_explosive`, `ammo_piercing`, `ammo_poison`, `ammo_slug`, `autofire`, `bipod`, `bonus_stats`, `bonus_team`, `flashlight`, `grenade_launcher`, `laser`, `second_sight`, `silencer`, `singlefire` |
| ver | Number | The version of the module. Version 2 should be used now. It does the following: Defaults pcs to an empty table \(use hidden=true to hide parts!\) and makes guess\_unit default to true |
| guess\_unit | Boolean | Guesses the unit for you. The resulted unit will be "units/mods/weapons/`id`/`id` Defaults to true with ver=2 |
| wpn\_pts | String | Like `guess_unit` but receives a weapon factory ID to put the parts in. The unit will be in: units/mods/weapons/`wpn_pts`\_pts/`id` |
| global\_value | String | For packs of mods, a global id assigned to all. A nice way to label your mods. You'll still have to create the global value through [GlobalValueModule](https://luffyyy.gitbook.io/beardlib/modules/globalvaluemodule) |
| override | Table | Overrides data of other attachments when this attachments in added to weapon |
| adds | Table | List of additional attachments added with this attachment |
| forbids | Table | List of attachments that can't be used with this attachments. Note. If attachment on list is currently on weapon it will be removed after adding this attachment |
| weapons | Table | A table of weapon ids \(factory ids\) that the weapon part can be added to |
| weapons\_override | Table | A table of weapons and the override they should have for the part |
| parts\_override | Table | A table of weapon mods and the override they should have for the part |
| parts\_forbids | Table | A table of weapon mods ids that can't work with the part |
| unit | String | Optional path to the first person unit. Defaults to units/mods/weapons/`id`/`id` You still need to add it through [AddFiles](https://luffyyy.gitbook.io/beardlib/modules/addfilesmodule) and all its dependencies such as object and material config |
| third\_unit | String | Optional path to the third person unit. You usually don't need a third person unit for weapon parts |
| stance\_mod | Table | A table of weapon ids -&gt; translation and rotation. Usually used by sights to set a custom stance for the weapon mod \(Lets you align the weapon with the sight to the center of the screen\) |
| name\_id | String | Localization ID for the title \(Defaults to bm_wp_ + `id`\) |
| texture\_bundle\_folder | String | Folder to contain the icon. The path will be guis/dlcs/`texture_bundle_folder`/textures/pd2/mods/icons/mods/`id`. Defaults to "mods" \(the last mods means weapon mods not mods as modding\) |
| is\_a\_unlockable | Boolean | Makes the item unlockable. Defaults to true |
| droppable | Boolean | Makes the item droppable through the end screen cards. Defaults to true |
| stats | Table | Stats added by attachment. Note, adding stats group wont remove other stats that are copied from based\_on. If you want remove copied stats from such attachments use value 0 \(Example damage="0"\).Available stats parameters and values: |
| perks | String/Table | A string or a table of perks that the part adds |
| a\_obj | String | Name of the bone used by the attachment |
| dlc | String | An optional DLC for the part. Defaults to "mods". Unless you know what you're doing, don't change this |
| sound\_switch | String | Changes firing sound prefix |

And any other tweakdata value. There are more that are not listed here yet.

#### The icon

The icon will be stored in guis/dlcs/mods/textures/pd2/blackmarket/icons/mods and will be named `id`.texture then add it using [AddFiles](https://luffyyy.gitbook.io/beardlib/modules/addfilesmodule).

### Example

This example is what you would put inside your main node within your [mod config](https://github.com/GreatBigBushyBeard/PAYDAY-2-BeardLib/wiki/Module-Config)

```markup
<WeaponMod id="wpn_part_YOUR_PART_NAME" based_on="wpn_fps_saw_m_blade_sharp" type="magazine" ver="2">
    <weapons>
        <value_node value="wpn_fps_saw"/>
        <value_node value="wpn_fps_saw_secondary"/>
    </weapons>
</WeaponMod>
```

This will add another saw blade to the saw. Based on an existing saw blade. You will still need to localize the title of the part. In this case `bm_wp_wpn_part_YOUR_PART_NAME` will need to be localized using [LocalizationModule](https://luffyyy.gitbook.io/beardlib/modules/localizationmodule). And add unit, object, material\_config, textures and icon through [AddFiles](https://luffyyy.gitbook.io/beardlib/modules/addfilesmodule) Thankfully shortcuts make it easier:

```markup
<AddFiles directory="assets">
    <texture path="guis/dlcs/mods/textures/pd2/blackmarket/icons/mods/wpn_part_YOUR_PART_NAME"/>    
    <unit_cc path="units/mods/weapons/wpn_part_YOUR_PART_NAME/wpn_part_YOUR_PART_NAME"/>
</AddFiles>
```

#### Template

Download template \(sight\) [here](https://minhaskamal.github.io/DownGit/#/home?url=https://github.com/ModWorkshop/BeardLib-Templates/tree/master/Sight-Template)

