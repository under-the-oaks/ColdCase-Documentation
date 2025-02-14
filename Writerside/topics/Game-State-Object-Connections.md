# Game State Object Connections

Because of the structure of our game state, describebd in [Game State Architecture](Game-State-Architecture.md), there
is a strongly defined hierarchy of objects. This document will explain how these objects interact with each other and
how they are connected.

## Add and Remove TileContents

The Map provides methods to find the coordinate of a Tile or TileContent in the TileArray. This coordinate can be used
to add or remove TileContents from Tiles. This is used to move TileContents around the Map by deleting them from one
Tile and adding them to another. The Tile and TileContent Abstract classes provide methods to push and pop TileContents
from the Tile by propagating the TileContent to be added or deleted to the TileContent referencing the target. This
TileContent then sets or removes its `tileContent` field.

## Rendering

Every frame, the game needs to render the current game state.
This happens through the call of render() from inside the LibGDX context. The call propagates to the Map class through
the Main and GameStage classes, because the Map is set as an actor of the GameStage and thus its render method is
called. The Map then calls the render method of all its Tiles, which in turn call the render method of their
TileContents. The TileContents then render themselves and their children.

Each tile and TileContent accesses the render batch independently with its own set of individual parameters, allowing
for sprite and gameplay specific render positions.

## Interaction System

The Map provides methods to find the coordinate of a Tile or TileContent in the TileArray. This is used by the
GameController to trigger interactions between TileContents as defined
in [](WIP-RAW-Interaction-System.md#4-interaction-flow). This action is always called on the Tile object on the
coordinate of the TileContent that is supposed to be interacted with. The Tile then calls the action method of the
TileContent, if it does not handle the action itself, it propagates the call to the TileContent on top of it. If no Tile
or TileContent can handle the action, it returns false to the GameController.

The same principle applies to the update method, which is called every time an action is handle (e.g. it returned true
in the GameController). The Map calls the update method of all its Tiles, which in turn call the update method of their
TileContents. The TileContents then update themselves and their children. This propagation is not stopped by handling
the action, so that all TileContents can react to the action.

## Deep Copy

The Map provides a method to create a deep copy of itself. This is used by the GameController to create Snapshots of the
game state. The deep copy method is called on the Map, which then calls the deep copy method of all its Tiles, which in
turn call the deep copy method of their TileContents. The TileContents then create a deep copy of themselves and their
children.
