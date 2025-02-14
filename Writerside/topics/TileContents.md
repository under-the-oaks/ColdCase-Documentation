# TileContents

## Description

Every object, wall or entity is considered a TileContent. This document will explain the different types of TileContents
and how they interact with each other.

## Types of TileContents

### Wall & InvisibleWall

A Wall is a TileContent that blocks the path of the player. The player cannot walk through a Wall, or interact with it.
The Wall also has an invisible counterpart.

### Spikes & Triggers

These are two different TileContents that interact with each other. When a player steps interacts with a Trigger, the
Spikes will toggle between being active and inactive. When the Spikes are active, the player will not be able to walk
over them. When the Spikes are inactive, the player can walk over them.

For this duo to work, the Trigger and the Spikes must be placed at the same coordinate in both worlds and the Player
interacting has to have the Glove item equipped.

<warning>
In the Implementation the Door and Door_Trigger are used as the Spikes and Trigger.
</warning>

### Goal

The Goal is the TileContent that the player has to reach to finish the level. For the Goal to be interactable both
players need to stand on a tile next to the Goal. Only then can one of the players interact with the Goal, loading the
next level for both players.

### Items

Items are considered TileContents inhabiting a tile when not picked up by either player. The player can walk over items
on the ground as well as interact with them to pick them up. Items can be used to unlock interact with other
TileContents such as the MovableBlock.

| Item  | Description                                                   | Interaction           |
|-------|---------------------------------------------------------------|-----------------------|
| Glove | Allows the player to interact with different physical objects | Trigger, MovableBlock |

### MovableBlock & MovableBlockTranscendent

The MovableBlock is a TileContent that can be pushed by the player. The player can push the MovableBlock in any
direction depending on the direction the player is facing when interacting with the block. Interactions between the
MovableBlock and Spikes is the same as with the player. The MovableBlock can be pushed over Spikes when they are
inactive, but not when they are active.

This TileContent exists in two different states: normal and transcendent. Transcendent MovableBlocks are present in both
game worlds and can be pushed by both players. The movement of the MovableBlock is synchronized between both players and
blocked by other non-passable TileContents in both worlds including the other player. The normal MovableBlock is only
present in one game world and can only be pushed by the player in that world.

### Hole

A Hole is a TileContent not passable by the player or other movable TileContents except for the MovableBlock. The
MovableBlock can be pushed into the Hole, making it disappear from the game world. This action makes the tile passable.

<note>
    Transcendent MovableBlocks can be pushed into a Hole in one game world making it disappear in one and logically
    transform into a wall in the other game world.
</note>

### Player

This TileContent represents the player. When interacted with it moves in a specified direction. This interaction is
controlled through the [`PlayerController`](Player.md).

### PortalObject

The PortalObject is a TileContent allowing the players to exchange items between their worlds. A player with an item
equipped can interact with this TileContent to send the item to the other player. For this to work in the game, there
has top be a PortalObject in both worlds at the same coordinates.

## Implementation Note

The TileContents are represented by an enum, which allows for easy access to the different TileContent types. Each Tile
Content Type contains an index and a reference to the TileContent class. The index is used to identify the TileContent
type when reading map data from a file.

| Tile Content                 | Index | Tile Content Class                                                                                                                                   |
|------------------------------|-------|------------------------------------------------------------------------------------------------------------------------------------------------------|
| Wall                         | 1     | [Wall](https://under-the-oaks.github.io/ColdCase-Client/tech/underoaks/coldcase/state/tileContent/Wall.html)                                         |
| Player                       | 3     | [Player](https://under-the-oaks.github.io/ColdCase-Client/tech/underoaks/coldcase/state/tileContent/Player.html)                                     |
| MovableBlock                 | 4     | [MovableBlock](https://under-the-oaks.github.io/ColdCase-Client/tech/underoaks/coldcase/state/tileContent/MovableBlock.html)                         |
| InvisibleWall                | 7     | [InvisibleWall](https://under-the-oaks.github.io/ColdCase-Client/tech/underoaks/coldcase/state/tileContent/InvisibleWall.html)                       |
| GloveItem                    | 8     | [GloveItem](https://under-the-oaks.github.io/ColdCase-Client/tech/underoaks/coldcase/state/tileContent/GloveItem.html)                               |
| GoalObject                   | 9     | [GoalObject](https://under-the-oaks.github.io/ColdCase-Client/tech/underoaks/coldcase/state/tileContent/GoalObject.html)                             |
| MovableBlockTranscendent     | 10    | [MovableBlockTranscendent](https://under-the-oaks.github.io/ColdCase-Client/tech/underoaks/coldcase/state/tileContent/MovableBlockTranscendent.html) |
| Door (Spike)                 | 12    | [Door](https://under-the-oaks.github.io/ColdCase-Client/tech/underoaks/coldcase/state/tileContent/Door.html)                                         |
| Door_Trigger (Spike_Trigger) | 13    | [Door_Trigger](https://under-the-oaks.github.io/ColdCase-Client/tech/underoaks/coldcase/state/tileContent/Door_Trigger.html)                         |
| Hole                         | 14    | [Hole](https://under-the-oaks.github.io/ColdCase-Client/tech/underoaks/coldcase/state/tileContent/Hole.html)                                         |
| PortalObject                 | 15    | [PortalObject](https://under-the-oaks.github.io/ColdCase-Client/tech/underoaks/coldcase/state/tileContent/PortalObject.html)                         |
    