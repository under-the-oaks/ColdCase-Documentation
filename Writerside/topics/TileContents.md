# TileContents

## Description

Every object, wall or entity is considered a TileContent. This document will explain the different types of TileContents
and how they interact with each other.

## Types of TileContents

### Wall

A Wall is a TileContent that blocks the path of the player. The player cannot walk through a Wall, or interact with it.

### Spikes & Triggers

These are two different TileContents that interact with each other. When a player steps interacts with a Trigger, the
Spikes will toggle between being active and inactive. When the Spikes are active, the player will not be able to walk
over them. When the Spikes are inactive, the player can walk over them.

For this duo to work, the Trigger must be placed on the same tile as the Spikes. And the Player interacting has to have
the Glove item equipped.

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

### MovableBlock

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