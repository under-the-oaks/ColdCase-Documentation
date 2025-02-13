# Connecting to a Custom Server

By default, **ColdCase** connects to its official server without requiring any configuration. However, if you need to use a custom server, follow these steps:

1. Navigate to the **ColdCase** installation directory.
2. Open the `.properties` file in a text editor.
3. Locate the following line:

   ```properties
   # Server address (exclude 'ws://')
   websocket_url=12.345.67.890:8081
   ```

4. Replace the IP and port with your custom server address.
5. Save the file and restart the game to apply the changes.

This will direct the game to connect to your specified server.