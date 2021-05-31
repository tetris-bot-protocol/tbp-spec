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
- `"uniform"`: Any piece may show up at any time with no or little bias.
- `"seven_bag"`: Pieces are drawn uniformly at random from a bag without
  replacement. When the bag is empty, it is refilled with one of each piece.
- `"general_bag"`: Pieces are drawn uniformly at random from a bag without
  replacement. When the bag is empty, it is reset to its filled state, which is
  specified in the `randomizer` object on the `start` message.

If the value of the `randomizer` attribute is not present or not recognized (as
may be the case if more randomizers are added in the future), the bot should
treat it as having the value `"uniform"`.

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

Attribute | Description
--------- | -----------
`bag`     | A list specifying the contents of the bag at the end of the queue. E.g. `["S", "L", "T"]`

### `general_bag` Object

Attribute     | Description
---------     | -----------
`current_bag` | A list specifying the contents of the bag at the end of the queue. E.g. `["I", "I", "O"]`
`filled_bag`  | A list specifying the filled state of the bag. E.g. `["I", "I", "O", "L", "J", "T"]`
