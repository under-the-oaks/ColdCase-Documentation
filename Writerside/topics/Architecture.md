# Overview

The COLD CASE Game Server is the multiplayer backend for the COLD CASE project.
It acts as a broker/relay server to facilitate real-time communication between players using a lobby system.
Each lobby supports up to two players using WebSocket connections.
The Server has no Gamelogic, it just relais the messages between the Clients in a Lobby.

## Platform

- **Runtime:** Built on [Deno](https://deno.land/), a secure runtime for JavaScript and TypeScript.
- **Port:** Listens on port **8080**.

## Lobby System

- **Lobby Identification:**
    - Each lobby is identified by a unique UID (generated via `crypto.randomUUID()`).
- **Client Management:**
    - Each lobby maintains a list of clients, where each client is represented by a WebSocket connection and a unique client ID.
    - the client id is not used in the logic and is only for debugging purposes. 
- **Capacity:**
    - Currently, each lobby supports up to **2 clients**.

## Creating a New Lobby

- **Request:**
  - A WebSocket connection is initiated with the URL query parameter: `session=new`.
- **Response:**
  - The server sends a JSON message containing the lobby UID:
    ```json
    {
      "class": "tech.underoaks.coldcase.remote.Messages$lobbyIdMessage",
      "lobbyId": "<GENERATED_LOBBY_UID>"
    }
    ```

## Joining an Existing Lobby

- **Request:**
  - A WebSocket connection is initiated with the query parameter: `session=<LOBBY_UID>`.
- **Response:**
  - The client is added to the lobby without an explicit confirmation message.
  - If the lobby does not exist or is full, the server terminates the connection with an appropriate error code.

