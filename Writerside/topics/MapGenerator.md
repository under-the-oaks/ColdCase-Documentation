# MapGenerator

The `MapGenerator` class is responsible for generating
maps ([Map](https://under-the-oaks.github.io/ColdCase-Client/tech/underoaks/coldcase/state/Map.html)) from text files
and converting them back to JSON format if needed. It provides methods for reading map data from structured text files,
parsing the information into a grid-based map representation, and serializing or deserializing map objects using JSON.

## Functionality

### Map Serialization from Text Files

The `serializeContentToMap(Path path, boolean isDetective)` method is responsible for reading and processing map data
from structured text files. These files contain different layers of information that define the map’s ground and tile
layers. The method converts this raw textual representation into a structured `Map` object, which can then be used in
the game.

#### Reading and Splitting the Map File

The method reads the map from a text file located in the specified directory. Depending on whether the map is for the
detective or the ghost, it selects either `map.detective` or `map.ghost`. The file is read line by line, and the
contents are stored. Errors are caught and logged if the file cannot be found or read.

The file has to be structured into multiple layers separated by `"---"`. These layers serve different purposes:

##### Metadata Layer

The first layer contains the dimensions of the map in the format:

   ```
   width height
   ```  

This information is extracted and used to initialize the map's `TileArray` size.

##### Tile Layer

The second layer defines the base tiles of the map using numerical indices, where each number
corresponds to a specific tile type. These numbers are processed and converted into `Tile` objects using the
[
`Tiles.getNewTileClassByIndex(int index)`](https://under-the-oaks.github.io/ColdCase-Client/tech/underoaks/coldcase/state/tiles/Tiles.html#getNewTileClassByIndex(int))
method.

##### Tile Content Layers

The subsequent layers define the content placed on top of the base tiles, such as
obstacles, interactive objects, or environmental elements. Similar to the tile layer, these values are processed and
assigned to their respective tile positions using the
[
`TileContents.getNewTileClassByIndex`](https://under-the-oaks.github.io/ColdCase-Client/tech/underoaks/coldcase/state/tileContent/TileContents.html#getNewTileClassByIndex(int))

#### Handling Tile Creation and Empty Tiles

Once the base tile layer is extracted, a two-dimensional `Tile` array is created to represent the map. If the number of
rows in the file does not match the specified dimensions, the method adjusts accordingly by reducing the map size or
filling in missing data with default values. If out-of-bounds access occurs due to inconsistent file formatting,
[`EmptyTile`'s](https://under-the-oaks.github.io/ColdCase-Client/tech/underoaks/coldcase/state/tiles/EmptyTile.html) (
`Tiles.getNewTileClassByIndex(0)`) are used as placeholders.

For the tile content layers, objects are added to each tile based on their corresponding indices. If a tile is marked as
an `EmptyTile`, it is typically treated as a barrier. For the player to not be able to walk on it, the method ads an
`InvisibleWall` on top of the `EmptyTile`. Special conditions ensure that border tiles receive appropriate UI
content markers.

#### Final Map Assembly

After all tiles and their contents have been processed, the method constructs a new `Map` object using the populated
`TileArray` and returns it. This finalized map structure represents the game environment and can be used in rendering,
and gameplay logic.

The process ensures that maps are dynamically created from simple text files while maintaining flexibility in their
design. It also introduces safeguards against improperly formatted data, ensuring stability in the game’s map loading
system.

### JSON Serialization and Deserialization

Once a map has been generated, it can be serialized into a JSON string using `serializeMapToJson(Map map)`, allowing for
saving and loading map configurations as JSON. Additionally, the `deserializeMap(String jsonMap)` method enables the
reconstruction of a `Map` object from a JSON string, facilitating persistence and data exchange.

<note>
This functionality is not used in the current version of the game. It was originally intended to support map saving and 
loading features during a game session.
</note>