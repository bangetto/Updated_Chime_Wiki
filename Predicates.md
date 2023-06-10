Predicates are user-defined conditions that must all be met for a model override to be used.

Consider the following example:
```json
{
  "parent": "minecraft:item/generated",
  "textures": {
    "layer0": "minecraft:item/wheat"
  },
  "overrides": [
    {
      "predicate": {
        "entity": {
          "hand": "either",
          "target_entity": {
            "id": "minecraft:cow",
            "nbt": {
              "Age": 1
            }
          }
        }
      },
      "model": "example:item/breeding_wheat"
    }
  ]
}
```
The override in this model will only be used if the entity holding the item has it in either of their hands, the entity the holder is looking at is a cow, and that cow is an adult.

### Predicates
| Name | Type | Description |
| --- | --- | --- |
| `count` | `Range` | The count of the item stack |
| `durability` | `Range` | The amount of damage on the item stack |
| `nbt` | `Nbt` | Nbt of the item stack |
| `name` | `String` | A string (or regex) to match the item's name against (must be a full match) |
| `hash` | `Object` | A JSON object with 3 values: `modulo` (an `int`), `value` (a `Range`), and `tag` (a subtag). These objects are used to create a hash of the provided subtag (or entire item stack tag if none is provided), modulo it by the provided value, and compare it against the provided range. This is useful for arbitrary but consistent randomized item models |
| `dimension/id` | `Identifier` | The id of the dimension the item is in |
| `dimension/has_sky_light` | `boolean`| Whether or not the dimension the item is in has sky light |
| `dimension/has_ceiling` | `boolean`| Whether or not the dimension the item is in has a bedrock ceiling |
| `dimension/ultrawarm` | `boolean`| Whether or not the dimension the item is in is considered ultrawarm, evaporating water that is placed and speeding up lava flow|
| `dimension/natural` | `boolean`| Whether or not the dimension the item is in has functioning compasses |
| `dimension/has_ender_dragon_fight` | `boolean`| Whether or not the dimension the item is in has an ender dragon fight |
| `dimension/piglin_safe` | `boolean`| Whether or not the dimension the item is in is safe for piglins to exist in |
| `dimension/bed_works` | `boolean`| Whether or not the dimension the item is in has functioning beds |
| `dimension/respawn_anchor_works` | `boolean`| Whether or not the dimension the item is in has functioning respawn anchors |
| `dimension/has_raids` | `boolean`| Whether or not the dimension the item is in has raids |
| `world/raining` | `boolean`| Whether or not it is raining in the world the item is in |
| `world/thundering` | `boolean`| Whether or not it is thundering in the world the item is in |
| `world/biome/id` | `Identifier`| The identifier of the biome the item is in |
| `world/biome/precipitation` | `enum`| The type of precipitation the biome the item is in has, one of `rain`, `snow`, or `none` |
| `world/biome/temperature` | `Range`| The temperature of the biome the item is in |
| `world/biome/downfall` | `Range`| The downfall of the biome the item is in |
| `entity/nbt` | `Nbt`| Nbt of the holding entity |
| `entity/x` | `Range`| The x position of the holding entity |
| `entity/y` | `Range`| The y position of the holding entity |
| `entity/z` | `Range`| The z position of the holding entity |
| `entity/light` | `Range`| The light level at the position of the holding entity's eyes |
| `entity/block_light` | `Range`| The block light level at the position of the holding entity's eyes |
| `entity/sky_light` | `Range`| The sky light level at the position of the holding entity's eyes |
| `entity/can_see_sky` | `boolean`| Whether or not the holding entity has a direct line of sight vertically to the sky not obstructed by solid blocks (anywhere where you can see rain) |
| `entity/hand` | `enum`| The hand the item is in, one of `main`, `off`, `either`, `neither` or `none` (`neither` and `none` are equivalent) |
| `entity/slot` | `enum` | The slot the item is in, one of `head`, `chest`, `legs`, or `feet` |
| `entity/target` | `enum`| The target of the current entity, one of `block`, `entity`, or `miss` |
| `entity/target_block/id` | `Identifier`| The id or a tag for the block the holding entity is looking at |
| `entity/target_block/can_mine` | `boolean`| Whether or not the held item is effective on the block the holding entity is looking at |
| `entity/target_entity/id` | `Identifier`| The id or a tag for the entity the holding entity is looking at |
| `entity/target_entity/nbt` | `Nbt`| The Nbt of the entity the holding entity is looking at |

### Types
#### `boolean`
Either `true` or `false`.

Example:

`"ultrawarm": true`

#### `Range`
A number or set of numbers to match against.

Example:
* `"x": 3`: matches exactly 3
* `"x": "<=5"`: matches every number that is less than or equal to 5
* `"x": "3..5"`: matches every number from 3 to 5 including 3 and 5
* `"x": "(3..5]`: matches every number from 3 to 5 including 5, but not 3

#### `Identifier`
A string representing a Minecraft identifier. Sometimes also can accept tags, prefixed with `#`.

Example:
* `"id": "minecraft:drowned"`: a mob with the id `minecraft:drowned`
* `"id": "drowned"`: a mob with the id `minecraft:drowned` (the `minecraft:` prefix is optional)
* `"id": "#minecraft:skeletons"`: a mob from the entity tag `minecraft:skeletons`

#### `enum`
A string from a list of values.

Example:
* `"hand": "either"`
* `"precipitation": "snow"`

#### `String`
A string or regex to match against fully, strings are treated as regex if they follow the format `/some/` (being surrounded by `/`), the `/` characters would be ignored.

Example:
* `"name": "/[0-9]+/"`: matches a name that only contains numbers
* `"name": "Hi"`: matches `Hi` exactly

#### `Nbt`
A json object representing fields to match, all numerical fields are parsed as ranges and all strings can be parsed as regex if in the format `/some/` (see above). A null value specifies that a field should not exist.

Example:

```json
"nbt": {
  "Health": "0..5"
}
```
Matches a player with 2.5 hearts or less

```json
"nbt": {
  "Enchantments": [
    {
      "id": "minecraft:fire_aspect"
    }
  ]
}
```
Matches an item enchanted with any level of fire aspect