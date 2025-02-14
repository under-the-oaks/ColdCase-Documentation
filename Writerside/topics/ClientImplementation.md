# Remote Game Controller System

## Table of Contents

1. [Introduction](#introduction)
2. [Architecture](#architecture)
3. [WebSocket Communication](#websocket-communication)
4. [Class Descriptions](#class-descriptions)
    - [WebSocketMessagesManager](#websocketmessagesmanager)
    - [RemoteGameController](#remotegamecontroller)
    - [Message and Messages](#message-and-messages)
5. [Message Types](#message-types)
6. [Error Handling and Synchronization](#error-handling-and-synchronization)

---

## Introduction

This system enables asynchronous communication between remote game instances using WebSockets.  
It manages remote interactions and Game State Updates (GSU) through a message-based approach.

---

## Architecture

The system consists of the following main components:

1. **WebSocketMessagesManager**
    - Manages pending requests and responses.
    - Sends and receives messages via WebSockets.

2. **RemoteGameController**
    - Creates and manages an interaction chain for remote game actions.
    - Synchronizes actions and processes messages.

3. **Messages Classes**
    - Defines various message types for communication.

4. **WebSocketClient**
    - implements the Websocket itself  (using Jakarta)
    - is responsible for creating and managing a session and holds the lobbyID

![RemoteColdCase.drawio.png](RemoteColdCase.drawio.png)



---

## WebSocket Communication

Communication is handled through WebSockets with JSON-formatted messages.  
Each request is identified by a unique `remoteGameControllerInstanceId`.

- **Sending a message:**
  - `toJson()` is profited by lib GDX
      ```java
      WebSocketClient.getInstance().send(json.toJson(new Messages.AppendRemoteInteractionMessage(...)));
      ```
- **receiving and processing a message:**
```java
    public static void handleIncomingMessages(Object deserializedObject) {
           switch (deserializedObject) {
               case Messages.AppendRemoteInteractionMessage messageObj -> {
                   // Process the message
               }
               //... for all message types 
           }
       }
 ```

## Class Descriptions

### WebSocketMessagesManager
**Responsibilities:**

- Manages pending callback futures for asynchronous responses.
- Sends and receives WebSocket messages.
- Routes incoming messages to appropriate handlers.

**Key Methods:**

`public void createRemoteInteractionChain(String remoteGameControllerInstanceId, CompletableFuture<Object> future)`
   - Creates a new remote interaction chain and stores the future object.

`public void callback(Message messageObj)`
   - Completes the future object when a response is received.

`public static void handleIncomingMessages(Object deserializedObject)`
   - Handles incoming WebSocket messages asynchronously.

### RemoteGameController 
**Responsibilities:**

- Creates remote interaction chains.
- Ensures synchronization using CompletableFuture.
- Sends actions and receives feedback.

**Key Methods:**
`public Queue<Interaction> triggerAction(Interaction interaction, boolean suppressTranscendentFollowUp)`
   - Triggers an interaction and waits for a response.

`public void applyGSUQueue()`
   - Applies pending Game State Updates.

### Message and Messages
**Message (Base Class):**
- Inherited by all message types.
```java
public abstract class Message {
    private String remoteGameControllerInstanceId;
}
```
### Messages (Various Message Types):
- Defines various message types for communication
```java
   public static class AppendRemoteInteractionMessage extends Message
   public static class CreateRemoteInteractionChainMessage extends Message
   public static class ApplyRemoteGSUsMessage extends Message
   public static class startGameMessage
   //...
```

### Message Types:

**AppendRemoteInteractionMessage**
   - Sends a request to append a remote interaction.
   - Fields:
     - interaction: The interaction to append.
     - suppressTranscendentFollowUp: Whether to suppress follow-ups.

**AppendRemoteInteractionResponseMessage**
   - Response message containing the result of appending a remote interaction.
   - Fields:
     - interactions: A queue of resulting interactions.

**CreateRemoteInteractionChainMessage**
   - Requests the creation of a remote interaction chain.
   - Fields:
     - remoteGameControllerInstanceId: Unique identifier for the controller.

**CreateRemoteInteractionChainResponseMessage**
   - Response to creating a remote interaction chain.
   - Fields:
     - remoteInteractionChainId: The newly assigned interaction chain ID.

**ApplyRemoteGSUsMessage**
   - Requests the application of all pending Game State Updates.
   - Fields:
     - remoteGameControllerInstanceId: Unique identifier.

**AbortRemoteGSUsMessage**
   - Requests to abort pending Game State Updates.
   - Fields:
     - remoteGameControllerInstanceId: Unique identifier.

**lobbyIdMessage**
   - Contains a lobby ID.
   - only used when a lobby is created with the keyword new as id as response
   - Fields:
     - lobbyId: The identifier of the game lobby.
     
**startGameMessage**
   - Description: Requests to start the game.
   - Fields:
     - levelIndex: The level to start.

**exitToMainMenuMessage**
   - Requests to return to the main menu.
   - Fields: None.

## Error Handling and Synchronization

**Timeout Handling:**
If no response is received within 3 seconds, the action is considered failed.
```java
try {
   Object returnObj = future.get(3, TimeUnit.SECONDS);
} catch (TimeoutException e) {
   System.err.println("TIMEOUT in triggerAction");
}
```


**Asynchronous Processing with CompletableFuture:**
Ensures that message processing does not block execution.
```java
CompletableFuture.runAsync(() -> {
    try {
        // Process message
    } catch (Exception e) {
        System.err.println("Error: " + e.getMessage());
    }
});
```

