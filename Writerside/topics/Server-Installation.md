# Server Installation

This document provides step-by-step instructions for installing and running the 
COLD CASE Game Server. It covers the prerequisites, installation steps and running the 
server locally.


## Prerequisites

Before you begin, ensure that you have the following installed on your system:

- **[Deno](https://deno.land/)** (version 2.0.0 or later)  
  Deno is required to run the server.
- **[Git](https://git-scm.com/)**  
  For cloning the repository from GitHub.

## Cloning the Repository

Clone the GitHub repository to your local machine using Git. Replace the repository URL with the correct one for your project.

```sh
git clone https://github.com/under-the-oaks/ColdCase-Server
```
```sh
cd ColdCase-Server
```

## Running the Server Locally
To start the server locally, use the following command:

```sh
cd server
```
```sh
deno run --allow-net --allow-read main.ts
```

After the server starts, you should see output indicating that it is listening on port 8080.

### Verifying the Server
**HTTP Request:**
Open your browser and navigate to http://localhost:8080 to view the served HTML file.

**WebSocket Connection:**
Use a WebSocket client (or browser-based tool) to connect to the server. For example, connect to:
ws://localhost:8080/?session=new
This will create a new lobby and allow you to test the WebSocket connection.