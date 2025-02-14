# Technical Details

This document provides an in-depth look into the implementation of the COLD CASE Game Server, covering server startup, connection handling, lobby management, client communication, and error handling.

## Server Startup and HTTP/WebSocket Handler

The server is initialized using Deno's `Deno.serve()` function on port **8080**. It distinguishes between regular HTTP requests and WebSocket upgrade requests. For example:

```typescript
Deno.serve({
  port: 8080,
  handler: async (request) => {
    // Check if the request is a WebSocket upgrade
    if (request.headers.get("upgrade") === "websocket") {
      const { socket, response } = Deno.upgradeWebSocket(request);
      // WebSocket handling logic is implemented below...
      return response;
    } else {
      // Serve the index.html file for HTTP requests
      const file = await Deno.open("./index.html", { read: true });
      return new Response(file.readable);
    }
  },
});
```
For WebSocket requests, the connection is upgraded to allow for persistent, bidirectional communication.

## Lobby Management
The server manages game lobbies using the following data structure:

```typescript
interface Lobby {
  clients: { socket: WebSocket; clientId: string }[];
  UID: string;
}

let lobbyList: Lobby[] = [];
```
## Creating a New Lobby
- **Trigger:** When the URL query parameter session=new is provided.
- **Process:**
  - A new lobby is created.
  - The first client is added to the lobby.
  - A unique lobby identifier (UID) is generated using crypto.randomUUID().
  - A JSON message is sent to the client containing the lobby UID.
  - Example code:

```typescript
const newLobby: Lobby = {
  clients: [{ socket, clientId }],
  UID: crypto.randomUUID(),
};
lobbyList.push(newLobby);

const message = {
  class: "tech.underoaks.coldcase.remote.Messages$lobbyIdMessage",
  lobbyId: newLobby.UID,
};

socket.send(JSON.stringify(message));
```

## Joining an Existing Lobby

- **Trigger:** When the URL query parameter session=<LOBBY_UID> is provided.
- **Process:**
  - The server searches for an existing lobby with the provided UID.
  - If the lobby is found and not full (i.e., fewer than 2 clients), the client is added to the lobby.
  - If the lobby does not exist or is full, the connection is terminated with an appropriate error code.
## Client Connection and Message Flow
### Connection Establishment
**When a client connects:**
- The socket.onopen event is triggered.
- The server extracts the session query parameter from the URL.
- Based on its value, the client either creates a new lobby or joins an existing one.
### Message Forwarding
- Once connected, any message sent by a client is forwarded to the other client(s) in the same lobby. For example:

```typescript
socket.onmessage = (event) => {
  console.log(`Received message from client ${clientId}: ${event.data}`);
  // Forward the message to all other clients in the lobby
  lobby.clients.forEach((client) => {
    if (client.socket !== socket) {
      client.socket.send(event.data);
    }
  });
};
```
This ensures that all participants within a lobby receive each other's messages.

## Error and Connection Handling

### Error Handling
If an error occurs on a client's WebSocket connection, the `socket.onerror` event is triggered. The server handles errors by:

- Logging the error.
- Removing the problematic client from the lobby.
- Closing the socket with an error code (e.g., 1007).
- Cleaning up the lobby if no clients remain.
- Example code:

