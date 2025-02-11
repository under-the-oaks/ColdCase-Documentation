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