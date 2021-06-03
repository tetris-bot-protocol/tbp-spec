# Overview

Currently, the Tetris Bot Protocol offers no way for the frontend to inform the
bot about fundamental patterns in the piece queue, such as the bag system that
modern Tetris games employ. This feature, called `randomizer`, adds a new
attribute to the `rules` message to indicate what system the game is using, and
a new attribute to the `start` message to indicate the state of the randomizer
at the end of the provided piece queue.

# Reference

## `rules` Message Additions

Attribute    | Description
---------    | -----------
`randomizer` | A string indicating which randomizer the game is using.

Possible `randomizer` values:
- `"uniform"`: Pieces are chosen randomly with little to no bias.
- `"seven_bag"`: Pieces are drawn uniformly at random from a bag without
  replacement. When the bag is empty, it is refilled with one of each piece.
- `"general_bag"`: Pieces are drawn uniformly at random from a bag without
  replacement. When the bag is empty, it is reset to its filled state, which is
  specified in the `randomizer` object on the `start` message.
- `"unknown"`: Fallback if no known randomizer is able to accurately represent
  the randomizer that the game uses.

If the value of the `randomizer` attribute is not present or not recognized (as
may be the case if more randomizers are added in the future), the bot should
treat it as having the value `"unknown"`.

## `start` Message Additions

Attribute    | Description
---------    | -----------
`randomizer` | An object specifying the state of the randomizer at the end of the queue.

The `randomizer` object has a `type` field indicating which randomizer it is.
This must be the same as the `randomizer` attribute on the corresponding `rules`
message. This object specifies the state of the randomizer at the end of the
queue.

### `uniform` Object

This object has no additional attributes.

### `seven_bag` Object

Attribute   | Description
---------   | -----------
`bag_state` | A non-empty list specifying the contents of the bag at the end of the queue. E.g. `["S", "L", "T"]`

### `general_bag` Object

Attribute     | Description
---------     | -----------
`current_bag` | An object specifying the number of each piece in the bag at the end of the queue. It is invalid for this to be zero for all pieces.
`filled_bag`  | An object specifying the number of each piece in the filled state of the bag. It is invalid for this to be zero for all pieces.

Example:
```
{
  "current_bag": {"I": 2, "O": 1, "L": 0, "J": 0, "S": 0, "Z": 0, "T": 0},
  "filled_bag": {"I": 2, "O": 1, "L": 1, "J": 3, "S": 0, "Z": 0, "T": 1}
}
```
