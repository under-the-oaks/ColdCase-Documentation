# Game State Architecture

Our game world is divided into a grid of tiles. Each tile can contain a multiple entity, which we call a [
`TileContent`](TileContents.md). The [`TileContent`](TileContents.md) is a base class that all entities in the game
world inherit from. This document will explain how the game state, containing all the [`TileContent`](TileContents.md)
objects, is managed and updated.

## Game State

The game state is a 2D array of tiles. Each tile can contain multiple [`TileContent`](TileContents.md) objects.

...