# Levels Enum

The `Levels` enum represents the different game levels available. Each enum constant is associated with a map path,
which is used to load the corresponding level's map.
## Method

### `getMapPath()`

- **Description:** Returns the file path of the selected map as a `String`.
    - **Usage Example:**
  ```java
    String mapPath = Levels.LEVEL_05.getMapPath();
  ```

## Enum Constants

- **LEVEL_05:** The first part of the tutorial.  
  Map Path: `"maps/Map_Tutorial_normalBlock"`

- **LEVEL_06:** The second part of the tutorial.  
  Map Path: `"maps/Map_Tutorial_transcendentBlock"`

- **LEVEL_07:** The third part of the tutorial.  
  Map Path: `"maps/Map_Tutorial_spikes"`

- **LEVEL_02:** Easy main level.  
  Map Path: `"maps/Map_Mvp"`

- **LEVEL_03:** Medium main level.  
  Map Path: `"maps/New_Level_Test_Medium"`

- **LEVEL_04:** Hard main level.  
  Map Path: `"maps/New_Level_Test_Hard"`