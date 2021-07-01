# Overview

"Garbage lines" are an integral feature to modern Tetris game modes, such as versus,
cheese race, and survival. Garbage lines are nearly-filled rows of cells which are added
directly to the bottom of the board. Bots need to be able to respond to these garbage
lines in order to avoid topping out. In many implementations, garbage is subject to rules
such as "change-on-attack", and sometimes their columns may be predicted. However, this
spec assumes garbage is entirely unpredictable and therefore may be added at any time,
with any frequency or density.

This feature adds a new message type to the TBP spec: `garbage`. This kind of message
should ideally be sent after a `play` message, but a bot should be able to handle it at
any time while it is calculating.

This feature is necessary so that a TBP bot may retain its internal state, such as random
bag position, when garbage lines appear. Without it, bot communication must be restarted
(by sending a new `start` message) if even a single garbage line appears.

# Reference

## `rules` Message Additions

Attribute | Description
--------- | -----------
`garbage` | A string indicating garbage behavior.

Currently, the only valid `garbage` value is `"general"`, indicating that garbage lines
may come in any shape or color. This attribute may be extended in the future to indicate
to the bot rules about garbage lines (such as change-on-attack, messiness, etc.).

## New Messages (Frontend -> Bot)

### `garbage`

The `garbage` message informs the bot about new garbage lines that have been added to the
board. It is only valid to send this message if the bot is calculating. This message
should cause the bot to start calculating new suggestions based on the newly added rows.

Attribute | Description
--------- | -----------
`rows`    | A list of lists of 10 board cells.

The cells in `rows` should be added directly to the bottom of the matrix, in order;
therefore the last row specified will become the new bottom of the matrix. Like the
`board` attribute of the `start` message, a cell can be `null` to indicate it is empty, or
a string to indicate it is filled. Filled cells ought to be `"G"` but this need not be
enforced by a bot.
