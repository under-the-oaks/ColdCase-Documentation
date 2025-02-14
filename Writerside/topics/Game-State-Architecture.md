# Game State Architecture

Our game world is divided into a grid of tiles. Each tile can contain an object, which we call a [
`TileContent`](TileContents.md). The [`TileContent`](TileContents.md) is a base class that all entities in the game
world inherit from. A list of all `TileContent`'s can be found [here](TileContents.md). This document will explain how
the game state, containing all the [`TileContent`](TileContents.md) objects, is managed and updated.

## Game State

The game state is held in a 2D array of type [`tile`](Tiles.md), called the [`TileArray`](Game-State-Architecture.md).
Each [`tile`](Tiles.md) can contain one [`TileContent`](TileContents.md) object. Each
[`TileContent`](TileContents.md) can contain another Instance of [`TileContent`](TileContents.md), allowing for multiple
objects on one tile. This achieves a hierarchical structure of the game world.

Accessed is this `TileArray` over the [`Map`](Game-State-Architecture.md) class, as shown in the class diagram below.

![ColdCaseMapClassDiagramm-Page-1.drawio.svg](ColdCaseMapClassDiagramm-Page-1.drawio.svg)

<warning>
The above class diagramm is simplified and does not show all methods and attributes of the classes. For a full overview
of the classes, please refer to the respective class documentation.
</warning>

### Class Documentation References

- [Map](https://under-the-oaks.github.io/ColdCase-Client/tech/underoaks/coldcase/state/Map.html)
- [Tile](https://under-the-oaks.github.io/ColdCase-Client/tech/underoaks/coldcase/state/tiles/Tile.html)
- [TileContent](https://under-the-oaks.github.io/ColdCase-Client/tech/underoaks/coldcase/state/tileContent/TileContent.html)

## **Summary of Class Interactions**

1. **`Map` contains multiple `Tile` objects**, forming the game grid.
2. **Specialized `Tile` types (`EmptyTile`, `GroundTile`)** inherit from `Tile`.
3. **Each `Tile` can hold one `TileContent`**, representing a new layer of the game world.
4. **Specialized `TileContent` types (`Wall`, `Player`, `Goal`)** inherit from `TileContent`.
5. **TileContent Objects interact through actions and updates**, supporting dynamic map changes (
   see [](WIP-RAW-Interaction-System.md)).