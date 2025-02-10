# Game State Architecture

Our game world is divided into a grid of tiles. Each tile can contain an object, which we call a [
`TileContent`](TileContent.md). The [`TileContent`](TileContent.md) is a base class that all entities in the game
world inherit from. A list of all `TileContent`'s can be found [here](TileContents.md). This document will explain how
the game state, containing all the [`TileContent`](TileContents.md) objects, is managed and updated.

## Game State

The game state is a 2D array of tiles, the [`TileArray`](TileArray.md). 

Each `tile` can contain one [`TileContent`](TileContents.md) object. Each [
`TileContent`](TileContents.md) can contain another Instance of [`TileContent`](TileContents.md).
![ColdCaseMapClassDiagramm-Page-2.drawio (1).svg](ColdCaseMapClassDiagramm-Page-2.drawio (1).svg)

Accessed is this `TileArray` over the [`Map`](Map.md) class. This class

![ColdCaseMapClassDiagramm-Page-1.drawio.svg](ColdCaseMapClassDiagramm-Page-1.drawio.svg)

### **1. `Map` Class**
The **`Map`** class is the core structure that manages the **game world**. It contains a **2D array of `Tile` objects** (`TileArray`), representing the grid-based map.

- The `Map` can **render** itself using the `render()` method, which takes a `Batch`, `originX`, and `originY` as parameters.
- It also provides a **deep copy functionality** via `deepClone()`, allowing for snapshot-based updates.

**Relationship:**
- `Map` contains multiple `Tile` objects (1 to 0..* relationship).

---

### **2. `Tile` Class**
Each **`Tile`** represents a single **grid element** in the `Map`. A tile contains:
- A **reference to `TileContent`**, which defines what is placed on the tile (e.g., an object, player, or structure).
- A **`Texture`** and **`Sprite`**, which visually represent the tile in the game.

Methods include:
- **Rendering** itself using `render()`.
- **Managing `TileContent`** by pushing (`pushTileContent()`) or popping (`popTileContent()`) content.
- **Cloning itself** with `clone()`.

**Relationship:**
- A `Tile` can have **zero or one** `TileContent` (1 to 0..1).

---

### **3. `TileContent` Class**
This class represents **interactive elements** placed on tiles (e.g., objects, walls, or the player).

Attributes:
- **`isPlayerPassable`** and **`isObjectPassable`** determine movement restrictions.
- **`Texture` and `Sprite`** for rendering.

Methods:
- **Rendering** itself with `render()`.
- **Handling updates** (`update()`), interacting with other elements (`action()`), and managing hierarchical content (`pushTileContent()`, `popTileContent()`).

**Relationships:**
- `TileContent` belongs to a `Tile` (0..1 relationship).
- Specialized into **`EmptyTile`**, `GroundTile`, `Wall`, and `Player` (inheritance).

---

### **4. Specialized `TileContent` Types**
The diagram shows several **specialized subclasses** of `TileContent`:
- **`EmptyTile`** – Represents an empty tile.
- **`GroundTile`** – Represents walkable ground.
- **`Wall`** – Represents an obstacle.
- **`Player`** – Represents the player character, capable of interactions.

These subclasses **inherit** the behavior of `TileContent` while adding specific properties or logic for different gameplay mechanics.

---

### **5. Interaction Mechanism**
The `TileContent` class contains an **`action()`** method that interacts with other elements using:
- **`InteractionChain`** – Manages interactions.
- **`Interaction`** – Defines a single interaction event.

Additionally, the `update()` method allows `TileContent` to modify its behavior in response to game events.

---

## **Summary of Class Interactions**

1. **`Map` contains multiple `Tile` objects**, forming the game grid.
2. **Each `Tile` can hold one `TileContent`**, representing objects, the player, or terrain.
3. **Specialized `TileContent` types (`Wall`, `Player`, `GroundTile`)** inherit from `TileContent`.
4. **Tiles and TileContent interact through actions and updates**, supporting dynamic map changes.
5. **The `InteractionChain` and `Interaction` classes manage interactions**, ensuring that objects respond to game mechanics.

This structured system allows for **flexible game mechanics**, supporting dynamic interactions, movement restrictions, and object updates within the **game world**. 🚀