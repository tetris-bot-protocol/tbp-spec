# Overview

We sometimes want to know the performance of a bot, and *node count*, *nodes created per second (nps)*, *depth*
are well-known good indexes to know that. This `move_info` feature will add an object to `suggestion` message that contains these
values.

# Reference

## `suggestion` Message Additions

Attribute   | Type     | Description
---------   | -------- | ----
`move_info` | `object` | An object which contains the move info.

A move_info is an object which **may** contain the following attributes:

Attribute | Type     | Description
--------- | -------- | ----
`nodes`   | `number` | Total number of nodes of current game tree.
`nps`     | `number` | Number of nodes created per second.
`depth`   | `number` | The depth of current game tree.
`extra`   | `string` | Additional informational string about the provided move.

Since bots may have different implementations of a game tree, it is not enforced for bots to provide all the values.
Also, it is up to bot developers how to count these values.

# Example

```json
{
  "type": "suggestion",
  "moves": [
    ...
  ],
  "move_info": {
    "nodes": 1312451,
    "nps": 568375,
    "depth": 8,
    "extra": "found pc in 3 moves"
  }
}
```