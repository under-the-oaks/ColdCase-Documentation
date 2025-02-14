# LevelManager

The LevelManager class is responsible for managing the game levels. It handles loading levels, 
tracking the current level, and transitioning between levels using the values provided by the 
Levels enum. This class implements the Singleton design pattern to ensure that only one instance 
exists throughout the game.


## Features 

- Singleton Pattern: Ensures that only one instance of LevelManager exists.
- Level Tracking: Maintains the index of the currently played level using the variable currentLevelIndex.
- Level Transitions: Loads the next level from the Levels enum and navigates to the main menu when the last level is completed.
- Integration: Collaborates with other components:
  - WebSocketClient: Closes the session when the last level is reached.
  - StageManager: Manages screen transitions and stage navigation.
  - PlayerController: Resets the player's inventory when a new level is loaded.

## Fields
public int currentLevelIndex
Description: Stores the index of the currently played level.

## Methods
### `static LevelManager getInstance()`
Returns the singleton instance of LevelManager. If no instance exists, a new one is created.

### `void loadNextLevel()`
Increments the currentLevelIndex and loads the corresponding level from the Levels enum. If the last level is reached, the index is reset and the main menu is displayed.
- Key Points:
  - Compares the updated index with Levels.values().length.
  - Uses WebSocketClient to close the session if the last level is reached.
  - Sets the next stage in StageManager to MAIN_MENU when all levels have been played.
### `void loadLevel(Levels level)`
Loads the specified level by resetting the player's inventory and instructing the StageManager to switch to the game stage (GAME).
