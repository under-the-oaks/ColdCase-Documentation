# Stage Management System

The [`StageManager`](https://under-the-oaks.github.io/ColdCase-Client/tech/underoaks/coldcase/stages/StageManager.html)
is responsible for handling different screens in the game, ensuring smooth transitions between menus, gameplay, and
settings. This system is built using LibGDX’s `Stage` and `Screen` classes.

## Screens and Stages in LibGDX

`Screen` and `Stage` classes are used to manage different parts of a game, such as menus, levels,
and UI interactions.

## Screen

The `Screen` interface represents a game state or scene. It defines lifecycle methods like `show()`,
`render()`, `resize()`, `hide()`, and `dispose()`, allowing developers to manage transitions between different parts of
the game. The Game class in LibGDX uses Screens to switch between different game states efficiently. Each screen can
contain logic for rendering, updating, and handling input. In ColdCase our Main Class implements the Game interface,
which comes with the Screen field.

## Stage

A `Stage` is a container for UI elements and actors that helps manage input and rendering. It simplifies
handling complex UI elements like buttons, labels, and animations. Stages use LibGDX’s Scene2D system, making it easier
to organize UI and game objects. We can add actors to a stage, update them, and let the
stage manage rendering and input processing.

<note>
For our game we combined both classes into  one Abstract class to get the benefits of both in one. See <a href="https://under-the-oaks.github.io/ColdCase-Client/tech/underoaks/coldcase/stages/AbstractStage.html">AbstractStage</a>.
</note>

## StageManager

The [`StageManager`](https://under-the-oaks.github.io/ColdCase-Client/tech/underoaks/coldcase/stages/StageManager.html)
class acts as the central controller for screen transitions. It allows the game to switch between different screens,
such as the main menu, gameplay, settings, and multiplayer connection screens.

It also initialises an InputMultiplexer. This allows both the [
`AbstractStage`](https://under-the-oaks.github.io/ColdCase-Client/tech/underoaks/coldcase/stages/AbstractStage.html) and
the `PlayerController`, which is part of an [
`MapActor`](https://under-the-oaks.github.io/ColdCase-Client/tech/underoaks/coldcase/stages/actors/MapActor.html), to
listen to input events. This way the users can interact with the game world as the player and the UI.

## Stages Enum

The `Stages` enum defines all possible game screens, including the main menu, gameplay, multiplayer hosting and
joining screens. These screens are implementations of the [
`AbstractStage`](https://under-the-oaks.github.io/ColdCase-Client/tech/underoaks/coldcase/stages/AbstractStage.html)
class, each with its own UI elements and
actors.

The `GameStage` class is the class responsible for the game world. It initializes the map, the inventory, and the pause
menu as separate actors (
[MapActor](https://under-the-oaks.github.io/ColdCase-Client/tech/underoaks/coldcase/stages/actors/MapActor.html),
[InventoryActor](https://under-the-oaks.github.io/ColdCase-Client/tech/underoaks/coldcase/stages/actors/InventoryActor.html),
[PauseMenu](https://under-the-oaks.github.io/ColdCase-Client/tech/underoaks/coldcase/stages/actors/PauseMenu.html)).
These actors initiate there respective UI and Game elements - the map, the inventory, and the pause menu. The MapActor
gets the correct level it is supposed to load from the [](LevelManager-md.md).

The `MainMenuStage` class is the class responsible for the main menu. It initializes the main menu buttons and their
click listeners. The buttons are created using the LibGDX Scene2D UI system. The main menu buttons are Host, Join,
Settings, and Exit. The Host button starts a new game session, the Join button connects to an existing game session and
the Exit button closes the game.

The `JoinStage` class is the class responsible for the multiplayer connection screen. It allows players to connect to a
game session using a session ID. The screen contains a text field for entering the session ID, a connection status
label, a connect button, and a back button. The connection status label shows whether the player is connected or not.
The connect button triggers the connection process, and the back button returns to the main menu.

The `HostStage` class is the class responsible for the host screen. It allows the host player to start a game session,
view the session ID, and manage the connection. The screen contains a session ID field, a copy button, a start button,
and a back button. The session ID field displays the session ID, the copy button copies the session ID to the clipboard,
the start button starts the game, and the back button returns to the main menu.

The `SettingsStage` class is supposed to be responsible for the settings screen. But do to a lack of time and there
being no settings to change, this screen is not implemented.

## AbstractStage

The [
`AbstractStage`](https://under-the-oaks.github.io/ColdCase-Client/tech/underoaks/coldcase/stages/AbstractStage.html)
class provides a common base for all stages in the game. It extends LibGDX’s `Stage` class and
implements the `Screen` interface, ensuring that every stage follows the same lifecycle. It is responsible for
rendering, input handling, and resizing. Each specific stage in the game must extend this class and implement the
`buildStage()` method, where UI elements and actors are set up. This method is called from the `StageManager` when a new
Stage is loaded. The `render` method is there to propagate the render call down to its actors (this in case of the
GameStage includes the Map), while `onConnected` and `onDisconnected` methods allow handling network events when
applicable.

## Interaction Between the Classes

The `StageManager` controls the active screen and ensures smooth transitions. When a screen change is requested, it
retrieves the correct screen from the `Stages` enum and sets it in the game. Each stage is an instance of
`AbstractStage`, which provides the rendering and input structure needed for the game. This system ensures that screen
management is modular and efficient, making it easy to add new screens or modify existing ones without disrupting the
core game loop.