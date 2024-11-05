# Game Data Structure and Communication Protocol

## Overview

In our game project, data related to the game map, tile types, and tile content will be stored and exchanged between the server and client using a structured JSON format. The data is stored in Redis, utilizing the JSON module for fast retrieval and update capabilities. This document outlines the JSON structure for storing game tiles, the protocol for server-client communication, and the usage of the `const` and `set` properties in the map data.

## Data Structure

### Tile Representation

Each tile in the game is represented by its x and y coordinates, structured as keys like `0_0`, `0_1`, and so on, where:

- The first number represents the x-coordinate.
- The second number represents the y-coordinate.

Tile data for each coordinate includes:
- `type`: Indicates the tile type (e.g., `groundTile`, `emptyTile`).
- `tileContent`: Contains nested items on a tile (e.g., pot, gloves, or flower).

#### Example
```json
"0_0": {
  "type": "groundTile",
  "tileContent": {
    "type": "pot",
    "tileContent": {
      "type": "flower"
    }
  }
},
"0_1": {
  "type": "groundTile",
  "tileContent": {
    "type": "gloves"
  }
}
```

### Server and Client Communication Structure

The JSON structure for server-client communication includes essential elements:

```json
{
  "connection": {
    "session_id": "7829c35789f357a89",
    "uid": "218557c9ffaa991",
  },
  "map": {
    "const": {
      "name": "test",
      "size_x": "2",
      "size_y": "2"
    },
    "set": {
      "0_0": {
        "type": "groundTile",
        "tileContent": {
          "type": "pot",
          "tileContent": {
            "type": "flower"
          }
        }
      },
      "0_1": {
        "type": "groundTile",
        "tileContent": {
          "type": "gloves"
        }
      },
      "1_0": {
        "type": "emptyTile"
      },
      "1_1": {
        "type": "groundTile"
      }
    }
  }
}
```

#### Key Components

- **connection**: Contains session-specific data, required with each request.
  - `session_id`: A unique identifier for the games’s session.
  - `uid`: A unique identifier for the user.
- **map**: Contains all data related to the game map.
  - `const`: Immutable data sent by the server at setup, defining:
    - `name`: The map name.
    - `size_x`, `size_y`: The dimensions of the map grid.
  - `set`: Used by both client and server to send tile updates. The `set` object can be modified to reflect real-time changes on the map tiles.

## Usage Scenarios

- **Initial Setup**: The server sends the `const` map data once at the beginning of a level to initialize the client’s map structure.
- **Tile Updates**: Both client and server use the `set` section to send updates, ensuring synchronized map state across sessions. Update occur in delta-format, only providing changed tiles.