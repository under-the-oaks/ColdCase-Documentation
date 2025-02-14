# Create and Integrate a New Level

This guide explains how to create and integrate a new level into the game. Follow the steps below to ensure the level is
properly recognized and loaded in the correct order.

## 1. Create a New Folder

Inside the project's `assets/maps` folder, create a new folder following the naming convention:

```
Map_<map-name>
```  

Replace `<map-name>` with your level's identifier. For example, if the new level is called "NewTutorial", the folder
should
be named `Map_NewTutorial`.

## 2. Create the Required Map Files

Inside the newly created folder, add two files:

- `map.detective`
- `map.ghost`

These files will define the level structure from the perspective of the **detective** and **ghost**.

## 3. Define the Level Layout

Edit both `map.detective` and `map.ghost` according to the map formatting guidelines provided by
the [MapGenerator](MapGenerator.md#reading-and-splitting-the-map-file).

Fill them with the desired level layout using the available Tiles and TileContents. For reference, see:
- [Available Tiles](Tiles.md#implementation-note)
- [Available TileContents](TileContents.md#implementation-note)

An example of a simple level layout is:

```
5 5
---
0 0 0 0 0
0 1 1 1 0
0 1 1 1 0
0 1 1 1 0
0 0 0 0 0
---
0 0 0 0 0
0 3 1 9 0
0 0 1 0 0
0 0 0 0 0
0 0 0 0 0
---
```

## 4. Register the Level in `Levels.java`

To ensure the game recognizes your new level, open the file:

```
tech/underoaks/coldcase/game/Levels.java
```  

Add a new entry to the Levels enum using the format:

```java
LEVEL_<number>(<map-path>),
```  

For example, if the new level is the 8th level added and the map path is `maps/Map_NewTutorial`, the entry would be:

```java
LEVEL_08("maps/Map_NewTutorial"),
```  

The order of levels in the `Levels` enum determines their loading sequence in a new play through. The first level listed (
Levels.values()[0]) is the one that loads first when starting a new game.

## Final Steps

After completing these steps, ensure that your level files are correctly formatted, and that the `Levels.java` file
compiles without errors. Your new level should now be integrated and ready to play!