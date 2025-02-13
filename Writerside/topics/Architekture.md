# Server-Client Architecture

## Overview
The system facilitates real-time communication between remote game clients using WebSockets. It enables interaction synchronization, game state updates, and event-driven messaging.

## Architecture Components

### 1. **Client**
Each client represents a game instance that can:
- Send interaction requests to the second client over the server.
- Receive interaction requests.
- Manage local game logic based on received messages.

#### **Key Components:**
- **RemoteGameController**
    - Handles remote interaction chains.
    - Sends game actions and listens for responses.
    - Applies or aborts game state updates.
- **WebSocketMessagesManager**
    - Sends and receives WebSocket messages.
    - Manages pending message callbacks using `CompletableFuture`.

### 2. **Server**
The server acts as a message relay and Lobby/Session manager.
- relays messages 
- create and manages session/Lobbys


## Communication Flow in an Lobby

![architecktur.md](architecktur.md)

1. **Client Sends Action Request**
    - A client triggers an action via `RemoteGameController`.
    - The action is sent as a `Messages.AppendRemoteInteractionMessage` via WebSockets.

2. **Server relays Action**
    - The server relay Message to second Client

3. **second client processes message and returns if action was valid**

2. **Server relays Action**
    - The server relay Message to back to first Client

4. **Client Updates Game State**
    - The client processes the response and apply actions if valid.
    - send Message to second Client to apply ore abort actionChain


