# Map Class

## Overview

The `Map` class represents the game world as a 2D array of `Tile` objects. It provides methods for retrieving,
modifying, rendering, and updating the game map. The class supports coordinate transformations, interaction handling,
and deep cloning for snapshot-based updates.

## Package

`tech.underoaks.coldcase.state`

## Dependencies

- `com.badlogic.gdx.graphics.g2d.Batch`
- `com.badlogic.gdx.math.Vector2`
- `tech.underoaks.coldcase.MapGenerator`
- `tech.underoaks.coldcase.game.Interaction`
- `tech.underoaks.coldcase.game.PlayerController`
- `tech.underoaks.coldcase.state.tileContent.UpdateTileContentException`
- `tech.underoaks.coldcase.state.updates.GameStateUpdateException`
- `tech.underoaks.coldcase.state.tileContent.TileContent`
- `tech.underoaks.coldcase.state.tiles.Tile`
- `java.nio.file.Files`, `java.nio.file.Path`
- `java.util.ArrayList`, `java.util.List`

---

## Fields

| Modifier  | Type       | Name            | Description                                       |
|-----------|------------|-----------------|---------------------------------------------------|
| `public`  | `Tile[][]` | `tileArray`     | A 2D array representing the game map grid.        |
| `private` | `boolean`  | `isSnapshotMap` | Indicates whether this map is a snapshot version. |

---

## Constructors

| Constructor               | Description                                                                                         |
|---------------------------|-----------------------------------------------------------------------------------------------------|
| `Map()`                   | Default constructor required for deserialization in [`MapGenerator`](Deserialization.md).           |
| `Map(Tile[][] tileArray)` | Creates a new `Map` instance with a predefined [`tileArray`](TileArray.md).                         |

---

## Methods

### **Tile Access Methods**

| Method                                  | Return Type | Description                                           |
|-----------------------------------------|-------------|-------------------------------------------------------|
| `Tile getTile(int x, int y)`            | `Tile`      | Retrieves the `Tile` at the specified coordinates.    |
| `Tile getTile(Vector2 position)`        | `Tile`      | Retrieves the `Tile` at the given `Vector2` position. |
| `void setTile(int x, int y, Tile tile)` | `void`      | Sets the `Tile` at the specified coordinates.         |
| `int getTileArrayWidth()`               | `int`       | Returns the width of the `tileArray`.                 |
| `int getTileArrayHeight()`              | `int`       | Returns the height of the `tileArray`.                |

---

### **Tile Content Operations**

| Method                                                            | Return Type   | Description                                                                     |
|-------------------------------------------------------------------|---------------|---------------------------------------------------------------------------------|
| `Vector2 getTileContentByType(Class<? extends TileContent> type)` | `Vector2`     | Finds the position of the first `TileContent` instance of the specified type.   |
| `int getChildIndex(Vector2 tile, TileContent tileContent)`        | `int`         | Retrieves the index of a specific `TileContent` child at a given tile position. |
| `TileContent getTileContentByIndex(Vector2 position, int index)`  | `TileContent` | Retrieves the `TileContent` at a given position and index.                      |

---

### **Map File Operations**

| Method                                                       | Return Type           | Description                                                                     |
|--------------------------------------------------------------|-----------------------|---------------------------------------------------------------------------------|
| `private static List<List<Integer>> readMapFile(Path path)`  | `List<List<Integer>>` | Reads a map file and returns a 2D list of integers representing the map layout. |
| `private static int getMatrixWidth(List<List<Tile>> matrix)` | `int`                 | Determines the maximum width of a matrix (the longest row).                     |

---

### **Rendering & Coordinate Transformations**

| Method                                                   | Return Type | Description                                                |
|----------------------------------------------------------|-------------|------------------------------------------------------------|
| `void render(Batch batch, float originX, float originY)` | `void`      | Renders the game map using isometric coordinates.          |
| `Vector2 isoTo2D(Vector2 pt)`                            | `Vector2`   | Converts an isometric coordinate to a 2D coordinate.       |
| `Vector2 twoDToIso(Vector2 pt)`                          | `Vector2`   | Converts a 2D coordinate to an isometric coordinate.       |
| `Vector2 twoDToIso45(int ex, int why)`                   | `Vector2`   | Converts 2D coordinates to a rotated isometric coordinate. |

---

### **Map Updates**

| Method                                                                                                                                                          | Return Type         | Description                                                                             |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------|-----------------------------------------------------------------------------------------|
| `void updateUntilStable(InteractionChain chain, Interaction interaction, TileContent handler) throws GameStateUpdateException, UpdateTileContentException`      | `void`              | Continuously updates the map until no further updates occur, preventing infinite loops. |
| `List<TileContent> updateMap(InteractionChain chain, Interaction interaction, TileContent handler) throws GameStateUpdateException, UpdateTileContentException` | `List<TileContent>` | Attempts to update each `Tile` in the `tileArray`.                                      |

---

### **Miscellaneous**

| Method                                                    | Return Type | Description                                       |
|-----------------------------------------------------------|-------------|---------------------------------------------------|
| `Map deepClone()`                                         | `Map`       | Creates a deep clone of the `Map` instance.       |
| `void dispose()`                                          | `void`      | Disposes of resources used by tiles.              |
| `boolean isSnapshotMap()`                                 | `boolean`   | Checks if the map is a snapshot map.              |
| `void setIsSnapshotMap(boolean snapshotMap)`              | `void`      | Sets whether the map is a snapshot.               |
| `static boolean isPlayerOnTile(Vector2 tilePosition)`     | `boolean`   | Checks if the player is on a given tile.          |
| `static boolean isPlayerNextToTile(Vector2 tilePosition)` | `boolean`   | Checks if the player is adjacent to a given tile. |

---