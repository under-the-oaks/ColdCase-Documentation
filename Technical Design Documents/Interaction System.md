# Interaction System

## Table of Contents

1. [Introduction](#1-introduction)
2. [System Overview](#2-system-overview)
3. [Architecture](#3-architecture)
   - 3.1 [Map](#31-map)
   - 3.2 [Tile](#32-tile)
   - 3.3 [TileContent](#33-tilecontent)
   - 3.4 [GameController](#34-gamecontroller)
   - 3.5 [Interaction Chains](#35-interaction-chains)
   - 3.6 [GameStateUpdates (GSUs)](#36-gamestateupdates-gsus)
4. [Interaction Flow](#4-interaction-flow)
   - 4.1 [Local Interactions](#41-local-interactions)
   - 4.2 [Transcendent Interactions](#42-transcendent-interactions)
5. [Networking and Synchronization](#5-networking-and-synchronization)
6. [Error Handling](#6-error-handling)
7. [Performance Considerations](#7-performance-considerations)
8. [Conclusion](#8-conclusion)

---

## 1. Introduction

This document outlines the technical design of the interaction system for an asynchronous multiplayer puzzle game. The game involves two players interacting across two different worlds to solve puzzles together. The interaction system is responsible for handling both basic movements and complex interactions, such as moving a block onto a pressure plate that triggers an action in the other player's world.

---

## 2. System Overview

The core of the interaction system revolves around the `GameController`, which manages interactions within a local instance of the `Map`. The `Map` is a 2D array containing `Tile` objects, each holding a `TileContent`. The `TileContent` represents objects placed upon a `Tile`, such as walls, players, pressure plates, and movable blocks. These contents can stack to represent multiple entities occupying the same tile (e.g., a player standing on a pressure plate).

The system distinguishes between three visibility states for `TileContent`:

- **Player One Only**: Visible and interactive only to player one.
- **Player Two Only**: Visible and interactive only to player two.
- **Transcendent**: Visible and interactive to both players.

---

## 3. Architecture

### 3.1 Map

- **Description**: Represents the game world as a 2D array of `Tile` objects.
- **Responsibilities**:
  - Maintains the state of all tiles and their contents.
  - Provides methods to access and modify tiles.

### 3.2 Tile

- **Description**: Represents a single position on the `Map`.
- **Properties**:
  - `TileContent content`: The content placed on the tile.
- **Responsibilities**:
  - Holds the `TileContent`.
  - Manages stacking of multiple `TileContent` objects.

### 3.3 TileContent

- **Description**: Abstract class representing any object placed upon a `Tile`.
- **Properties**:
  - `TileContent nextContent`: Reference to the next content in the stack.
  - `VisibilityState visibilityState`: Indicates visibility to players.
- **Methods**:
  - `action()`: Defines the action to be performed when interacted with.
  - `update()`: Updates the state based on interactions.
- **Responsibilities**:
  - Represents game entities like walls, players, pressure plates, and movable blocks.
  - Handles interactions and updates.

### 3.4 GameController

- **Description**: Central manager on each client responsible for handling interactions.
- **Properties**:
  - `Map currentMap`: The active game map.
  - `Queue<GameStateUpdate> gsuQueue`: Queue of GSUs to apply.
  - `Snapshot snapshotMap`: Copy of the map used for simulating interactions.
- **Methods**:
  - `createSnapshot()`: Creates a snapshot of the current `Map`.
  - `appendGSU(GameStateUpdate gsu)`: Adds a GSU to the queue.
  - `applyGSUs()`: Applies all GSUs in the queue to the active game state.
  - `abortInteractionChain()`: Discards the snapshot and clears the GSU queue.
- **Responsibilities**:
  - Manages interaction chains and validates actions.
  - Coordinates transcendent interactions with the remote client.
  - Provides feedback on invalid actions.

### 3.5 Interaction Chains

- **Description**: Sequences of interactions resulting from a player's action.
- **Flow**:
  1. Player initiates an action.
  2. `GameController` creates a snapshot of the current `Map`.
  3. Actions are simulated on the snapshot.
  4. GSUs are appended to the `gsuQueue`.
  5. Additional interactions triggered by the initial action are processed.
  6. If all actions are valid, GSUs are applied to the active game state.
- **Responsibilities**:
  - Ensures interactions are error-free before applying them.
  - Manages cascading effects of interactions.

### 3.6 GameStateUpdates (GSUs)

- **Description**: Abstract class representing atomic changes to the game state.
- **Types**:
  - **Map Modification**: Changes to the map structure (e.g., moving an object).
  - **Visual Effects**: Particle effects or animations.
  - **Audio Effects**: Sound effects corresponding to actions.
- **Properties**:
  - `updateType`: The type of update.
  - `updateData`: Data required to perform the update.
- **Responsibilities**:
  - Encapsulates changes to be made to the game state.

---

## 4. Interaction Flow

### 4.1 Local Interactions

**Process**:

1. **Action Request**: The player requests a new interaction chain via the `GameController`.
2. **Snapshot Creation**: The `GameController` creates a snapshot of the current `Map`.
3. **Action Execution**: The player requests to append an interaction to a `TileContent`.
   - The `GameController` calls the `action()` method of the target `TileContent`.
   - The `TileContent` appends necessary GSUs using `appendGSU()`.
4. **GSU Queue Update**: The `GameController` adds the GSUs to the `gsuQueue`.
5. **Trigger Check**: The `GameController` checks for additional triggered actions.
   - If additional `TileContents` need updating, their `update()` methods are called.
6. **Validation**: If no errors occur during simulation, the interaction chain is valid.
7. **Application**: The player invokes `applyGSUs()`, applying all GSUs to the active `Map`.
8. **Feedback**: Success or failure is communicated to the player through visuals or sounds.

**Example**:

- **Action**: Player moves a block onto a pressure plate.
- **Flow**:
  - The block's `action()` moves it onto the pressure plate.
  - The pressure plate's `update()` triggers a door to open.
  - All actions are validated in the snapshot before applying.

### 4.2 Transcendent Interactions

**Process**:

1. **Action Request**: The player initiates an action on a transcendent `TileContent`.
2. **Snapshot Creation**:
   - Local `GameController` creates a local snapshot.
   - Via API, requests the remote client to create a snapshot.
3. **Local Action Execution**:
   - The local `TileContent`'s `action()` method is called.
   - GSUs are appended locally.
4. **Remote Action Execution**:
   - The local `GameController` requests the remote client to append a remote interaction.
   - The remote `GameController` calls the corresponding `action()` method on its snapshot.
   - GSUs are appended on the remote client.
5. **Validation**:
   - Both clients validate their interaction chains.
   - If successful, both proceed; if not, the process aborts.
6. **Application**:
   - Local client calls `applyGSUs()`.
   - Signals the remote client via API to apply its GSUs.
7. **Abort Mechanism**:
   - If errors occur, the local client calls `abortRemoteInteractionChain()`.
   - Both snapshots are discarded, and GSUs are cleared.
8. **Feedback**: Players are informed of the success or failure of the interaction.

**Example**:

- **Action**: Moving a transcendent block that affects both worlds.
- **Flow**:
  - Local block movement is simulated.
  - Remote block movement is requested and simulated.
  - Both clients apply the GSUs if no errors occur.

---

## 5. Networking and Synchronization

- **Server Framework**: Deno server using WebSockets for real-time communication.
- **API Endpoints**:
  - `createRemoteInteractionChain()`: Instructs the remote client to create a snapshot.
  - `appendRemoteInteraction(action)`: Requests the remote client to execute an action.
  - `applyRemoteGSUs()`: Signals the remote client to apply its GSUs.
  - `abortRemoteInteractionChain()`: Instructs the remote client to discard its snapshot.
- **Data Transmission**:
  - Uses JSON or binary format for efficiency.
  - Includes action types, parameters, and state information.
- **Synchronization Strategy**:
  - Ensures both clients have consistent game states after transcendent interactions.
  - Minimizes latency impact by limiting network calls to essential interactions.
- **Failure Handling**:
  - Network failures trigger abort mechanisms to maintain game state integrity.
  - Retries or error messages are implemented as needed.

---

## 6. Error Handling

- **Invalid Interactions**:
  - Detected during simulation on the snapshot.
  - Causes the snapshot to be discarded.
  - The action is marked invalid, and the player is notified.
- **Remote Interaction Failures**:
  - If the remote client reports an unsuccessful interaction, both clients abort the chain.
  - The local client invokes `abortRemoteInteractionChain()`.
- **User Feedback**:
  - Visual cues (e.g., flashing tiles, error messages).
  - Audio cues (e.g., error sounds).
- **Logging and Debugging**:
  - Errors are logged locally for debugging purposes.
  - Network communication errors are logged with timestamps and details.

---

## 7. Performance Considerations

- **Client Framework**: Using LibGDX for efficient rendering and game logic execution.
- **Optimization Techniques**:
  - **Local Interactions**: Processed without server communication for minimal latency.
  - **Transcendent Interactions**: Network calls are minimized and optimized.
  - **Data Structures**: Efficient use of arrays and object pooling to reduce memory overhead.
- **Asynchronous Processing**:
  - Network communication is handled asynchronously to prevent blocking the main game thread.
- **Latency Management**:
  - Anticipate and handle network delays gracefully.
  - Implement timeouts and retries for network calls.
- **Resource Management**:
  - Limit the number of transcendent objects to reduce synchronization overhead.
  - Optimize textures and assets for better performance on various hardware.

---

## 8. Conclusion

The designed interaction system provides a robust framework for handling complex player interactions in an asynchronous multiplayer environment. By utilizing snapshots and interaction chains, the system ensures that all actions are validated before affecting the active game state. The differentiation between local and transcendent interactions allows for efficient processing and minimal network dependency, enhancing overall game performance and player experience.

---

**Note**: This design assumes that security and cheat prevention are not priorities for this project, as specified. However, should these concerns arise in the future, additional measures such as server-side validation and encrypted communications may be necessary.
