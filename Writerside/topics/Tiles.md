# Tiles

## Description

Tiles are the building blocks of the game world and represent the ground or lack thereof. Each tile can contain a single
TileContent, which the player can interact with. The player can walk on any tile that does not contain a Wall or Hole.

## Types of Tiles

### Ground Tile

The Ground Tile is the default tile type. The player can walk on the Ground Tile without any restrictions.

### Empty Tile

The Empty Tile is a tile that represents a empty space in the game world, without any functionality. The player should
not be able to walk on an Empty Tile.

## Implementation Note

The Tiles are represented by an enum, which allows for easy access to the different tile types. Each Tile Type contains
an index and a reference to the Tile class. The index is used to identify the tile type when reading map data from a
file.

| Tile Type | Index | Tile Class                                                                                                         |
|-----------|-------|--------------------------------------------------------------------------------------------------------------------|
| Empty     | 0     | [EmptyTile](https://under-the-oaks.github.io/ColdCase-Client/tech/underoaks/coldcase/state/tiles/EmptyTile.html)   |
| Ground    | 1     | [GroundTile](https://under-the-oaks.github.io/ColdCase-Client/tech/underoaks/coldcase/state/tiles/GroundTile.html) |