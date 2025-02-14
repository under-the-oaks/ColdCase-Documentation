# Interaction System

## Table of Contents

1. [Introduction](#1-introduction)
2. [System Overview](#2-system-overview)
3. [Architecture](#3-architecture)
    - 3.1 [Map](#3-1-map)
    - 3.2 [Tile](#3-2-tile)
    - 3.3 [TileContent](#3-3-tilecontent)
    - 3.4 [GameController](#3-4-gamecontroller)
    - 3.5 [InteractionChain](#3-5-interactionchain)
    - 3.6 [GameStateUpdates (GSUs)](#3-6-gamestateupdates-gsus)
    - 3.7 [Snapshot](#3-7-snapshot)
4. [Interaction Flow](#4-interaction-flow)
    - 4.1 [Local Interactions](#4-1-local-interactions)
    - 4.2 [Transcendent (Remote) Interactions](#4-2-transcendent-remote-interactions)
5. [Networking and Synchronization](#5-networking-and-synchronization)
6. [Error Handling](#6-error-handling)
7. [Performance Considerations](#7-performance-considerations)
8. [Diagram](#8-diagram)

---

## 1. Introduction

This document provides a mid- to high-level overview of the Coldcase interaction system. It describes how player interactions are processed—both locally and across clients—to produce a consistent game state in an asynchronous multiplayer environment. The system validates actions using snapshots and chains of updates before committing changes. While it complements the detailed Javadoc, it also explains key architectural decisions and interaction flows.

---

## 2. System Overview

At its core, the interaction system centers on a single, centralized controller (the **GameController**) that manages the active game map, processes interaction requests, and coordinates both local and remote updates. The game world is modeled as a 2D array of **Tile** objects, where each tile holds a stack of **TileContent** entities. Actions initiated by the player result in the creation of an **InteractionChain**—a temporary, simulated copy (via a **Snapshot**) of the game state. This chain gathers a series of **GameStateUpdates (GSUs)** before the validated changes are applied to the active map.

Key aspects include:
- **Local Interactions:** Actions processed and validated using local snapshots.
- **Transcendent (Remote) Interactions:** Actions that affect both players’ views are coordinated with a remote counterpart through a **RemoteGameController**.

---

## 3. Architecture

### 3.1 Map

- **Purpose:** Represents the game world as a 2D array of `Tile` objects.
- **Responsibilities:**
    - Provide access to individual tiles (e.g., `Tile getTile(int x, int y)` or `Tile getTile(Vector2 pos)`).
    - Manage map updates and deep cloning (used in creating snapshots).

### 3.2 Tile

- **Purpose:** Encapsulates a single position in the game world.
- **Responsibilities:**
    - Store a stack of `TileContent` entities.
    - Support rendering via methods such as `render(Batch batch, float x, float y)`.
    - Manage tile content with methods like `pushTileContent()`, `popTileContent()`, and `topTileContent()`.

### 3.3 TileContent

- **Purpose:** Abstract base class for any entity that occupies a tile.
- **Key Methods/Signatures:**
    - `public abstract boolean action(InteractionChain chain, Interaction interaction) throws GameStateUpdateException;`
    - `public abstract boolean update(InteractionChain chain, Vector2 tilePosition, Interaction interaction, TileContent handler) throws GameStateUpdateException, UpdateTileContentException;`
- **Responsibilities:**
    - Represent game objects (e.g., players, blocks, walls).
    - Provide recursive handling of actions and updates through methods such as `handleAction()` and `handleUpdate()`.
    - Support stacking of multiple contents on a single tile.

### 3.4 GameController

- **Purpose:** Central manager that processes interactions within the game.
- **Key Characteristics:**
    - **Singleton Pattern:** Accessed via `GameController.getInstance()`.
    - **Core Methods/Signatures:**
        - `public boolean triggerAction(Interaction interaction)`
        - `public static TileContent triggerLocalAction(InteractionChain chain, Interaction interaction)`
    - **Responsibilities:**
        - Create and manage snapshots via **Snapshot**.
        - Maintain a stack of **InteractionChain** objects and a queue of pending GSUs.
        - Coordinate local interactions and trigger remote actions (using **RemoteGameController**) when a transcendent action occurs.
        - Validate and apply updates only after all interactions in the chain have been successfully simulated.

### 3.5 InteractionChain

- **Purpose:** Represents a sequence of interactions initiated by a player’s action.
- **Key Methods/Signatures:**
    - `public void addAction(Interaction interaction)`
    - `public Queue<GameStateUpdate> getGSUQueue()`
- **Responsibilities:**
    - Hold a snapshot of the game state (via **Snapshot**) for safe simulation.
    - Accumulate pending local and remote actions.
    - Gather GSUs that encapsulate atomic changes to be applied if the entire interaction chain is valid.

### 3.6 GameStateUpdates (GSUs)

- **Purpose:** Abstract class representing an atomic update to the game state.
- **Key Methods/Signatures:**
    - `public abstract void apply(Map map)`
- **Responsibilities:**
    - Encapsulate changes (such as moving an object or triggering visual/audio effects).
    - Be queued during an interaction chain and applied only if all interactions pass validation.
- **Note:** Implementations such as `MoveUpdate` serve as concrete realizations, though detailed internals (e.g., rotation changes for players) are abstracted here.

### 3.7 Snapshot

- **Purpose:** Provides a deep-cloned copy of the current game state.
- **Responsibilities:**
    - Allow for safe simulation of interactions without immediately affecting the active game state.
    - Be used by the **InteractionChain** to validate complex or cascaded interactions.

---

## 4. Interaction Flow

The interaction flow is the core mechanism that processes player actions, ensuring that only fully validated and consistent updates are applied to the game state. The system simulates each action on a deep-cloned snapshot of the current map, accumulates a chain of updates, and only commits those changes once all cascading effects have been successfully validated. This process is divided into two main parts: local interactions and transcendent (remote) interactions.

### 4.1 Local Interactions

**Overview:**  
Local interactions simulate and validate a player’s action within a deep-cloned snapshot of the game state. This ensures that no changes are committed to the active map until the complete chain of effects (including cascading updates) is confirmed as valid.

**Detailed Steps:**

1. **Action Request Initiation:**
    - When a player initiates an action, the system calls the `GameController.triggerAction(Interaction interaction)` method.
    - A new **InteractionChain** is created. This chain encapsulates a deep-cloned snapshot of the active **Map**, ensuring that simulations are isolated from the actual game state.

2. **InteractionChain Management:**
    - The GameController uses a stack of InteractionChains to handle nested or cascading actions.
    - The new InteractionChain is pushed onto this stack, providing a context for any additional actions triggered by the initial event.

3. **Local Action Execution on the Snapshot:**
    - The system invokes the `triggerLocalAction(InteractionChain chain, Interaction interaction)` method.
    - **Target Resolution:**
        - The snapshot is queried for the target **Tile** using the coordinates provided in the Interaction.
        - The corresponding **TileContent** is retrieved from the tile. If the tile contains stacked contents, the call to `handleAction(InteractionChain, Interaction)` propagates recursively until the proper handler is found.
    - **Action Simulation:**
        - The `action()` method of the identified TileContent is executed. This simulates the action (for example, moving an object or stepping onto a pressure plate) without altering the active game state.

    <note>
    Details on how <b>Tiles</b> are handling interactions can be found <a href="Game-State-Object-Connections.md">here</a>.
    </note>

4. **Cascading Updates and Stability Check:**
    - Many interactions cause additional updates (e.g., a pressure plate might trigger a door to open).
    - The system processes these subsequent effects by calling `update()` or `handleUpdate()` on the affected TileContents.
    - To ensure that all cascading effects are captured, the method `updateUntilStable()` is executed. This method repeatedly applies updates until no further changes occur or a maximum iteration threshold is reached (to prevent infinite loops in the case of cyclic dependencies).
    - As each valid change is simulated, a corresponding **GameStateUpdate (GSU)** is generated and added to the InteractionChain’s GSU queue.

5. **Validation and Accumulation:**
    - Once the simulation stabilizes and all cascading interactions are processed, the InteractionChain holds a complete, validated queue of GSUs.
    - If any part of the chain fails (due to validation errors, timeouts, or exceptions), the entire chain is aborted, ensuring no partial or inconsistent updates are applied.
    - When successful, the pending GSUs from the InteractionChain are merged into the GameController’s update queue. These GSUs are later applied to the active map, thereby committing the changes.

### 4.2 Transcendent (Remote) Interactions

**Overview:**  
Transcendent interactions extend the local interaction process to actions that affect both players’ game states. This requires synchronizing the local simulation with a remote counterpart to ensure consistency across all clients.

**Detailed Steps:**

1. **Detection of Transcendence:**
    - After executing a local action, the system checks the visibility state of the target TileContent.
    - If the content is marked as **TRANSCENDENT**, the action must be replicated on the remote client.

2. **Remote Action Triggering:**
    - The GameController calls upon the **RemoteGameController** to trigger the corresponding action remotely.
    - **Key Method Signature:**  
      `public Queue<Interaction> triggerAction(Interaction interaction, boolean suppressTranscendentFollowUp)`  
      The `suppressTranscendentFollowUp` flag prevents endless loops by avoiding recursive remote calls.
    - A future-based synchronization mechanism (with a timeout) is used to wait for the remote response.

3. **Remote Snapshot and InteractionChain:**
    - On the remote client, a similar process unfolds: a new InteractionChain is created with its own snapshot of the game state.
    - The remote GameController processes the action in a manner identical to the local system, generating its own queue of GSUs and handling any cascading effects.
    - The remote response returns a queue of pending interactions (if any) that need to be integrated into the local chain.

4. **Synchronization and Merging:**
    - After both local and remote simulations are completed, the system validates that the overall interaction chain is consistent across clients.
    - The remote GSUs and any additional pending remote interactions are merged into the local InteractionChain.
    - Only if both sides validate the full chain does the system proceed with applying the updates.

5. **Finalization and Application:**
    - Once synchronization is confirmed, both local and remote GameControllers apply their respective GSUs to the active game state.
    - This step finalizes the interaction, ensuring that the state is updated in a synchronized fashion across all connected clients.

6. **Error Recovery and Abortion:**
    - If an error occurs at any point during remote processing (for example, a timeout or an invalid update), the local GameController will invoke the remote abort mechanism.
    - The **RemoteGameController.close()** method is called to abort pending remote GSUs, and the entire interaction chain is discarded to maintain state consistency.

**Key Signatures Involved:**
- **Local Trigger:**  
  `public boolean triggerAction(Interaction interaction)`
- **Local Handling:**  
  `public static TileContent triggerLocalAction(InteractionChain chain, Interaction interaction)`
- **Remote Trigger:**  
  `public Queue<Interaction> triggerAction(Interaction interaction, boolean suppressTranscendentFollowUp)`

![GameController.svg](GameController.svg)

---

## 5. Networking and Synchronization

The interaction system uses WebSocket communication brokered by a Deno server. The **RemoteGameController** class is responsible for handling remote interaction chains. Key aspects include:
- **Remote Action Triggering:**
    - Method signature: `public Queue<Interaction> triggerAction(Interaction interaction, boolean suppressTranscendentFollowUp)`
    - Synchronization is handled through a future-based mechanism with defined timeouts.
- **GSU Application:**
    - Remote updates are applied via `applyGSUQueue()`.
- **Cleanup:**
    - On completion or error, `close()` is called to abort pending remote updates.

*Note: The remote interaction system will be documented in more detail in its own topic.*

---

## 6. Error Handling

Error management is integrated into both local and remote flows:
- **Local Interactions:**
    - Exceptions such as `UpdateTileContentException`, `GameStateUpdateException`, and `TimeoutException` are caught during simulation.
    - On error, the snapshot is discarded, the interaction chain is aborted, and appropriate feedback (e.g., visual cues) is generated.
- **Remote Interactions:**
    - Timeouts and execution errors are similarly handled.
    - In case of failure, the local controller invokes remote abort methods (via **RemoteGameController.close()**) to ensure state consistency.
- **Logging:**
    - Errors are logged (with timestamps and stack traces) to aid in debugging and system stability.

---

## 7. Performance Considerations

The system is optimized for responsiveness:
- **Local Processing:**
    - Interactions are simulated on snapshots to minimize latency.
    - Chained actions and GSUs are processed asynchronously.
- **Remote Efficiency:**
    - Network calls are minimized and use a future-based synchronization to prevent blocking.
- **Resource Management:**
    - Use of efficient data structures (e.g., stacks for interaction chains, queues for GSUs).
    - Object pooling and deep cloning techniques reduce memory overhead.
- **Latency Handling:**
    - Timeouts and retries are built into the remote synchronization process to address potential network delays.

---

## 8. Class-Diagram

![Class-Diagram of GameController](GameController_2.svg)

