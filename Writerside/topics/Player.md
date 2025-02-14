# Player

The player can move, interact with objects and manage his inventory. Movement is controlled using the W, A, S, and D
keys, allowing the player to navigate in four directions. If the player holds down a movement key, they will continue
moving automatically after a short delay. The player's direction is updated based on movement, and their character
sprite changes accordingly.

## Interacting with the Environment

By pressing the E key, the player can interact with objects in front of them. This includes picking up items, pushing
blocks or interacting with spike triggers. The interaction system is tied to the player’s position and facing
direction, ensuring that they can only interact with objects directly in front of them.

Some interactions also require specific items to be equipped. For example, the player needs to have the Glove item
equipped to push blocks or toggle spike triggers. If the player does not have the required item, no interaction will
occur.

## Managing the Inventory

The player stores picked up items in an inventory with a singular inventory slot. The item is then automatically used in
all further interactions. In the UI the item is displayed in the inventory slot in the bottom left corner of the screen.

## Responsive and Automatic Movement

The game ensures that movement feels smooth and responsive. If the player holds a movement key, they will continue
moving in that direction at regular intervals. If the key is released, movement stops immediately. This system is
inspired by classic tile-based movement systems, like the one the early Pokémon games used. It is designed to be simple
and intuitive, allowing players to focus on solving puzzles and exploring the game world without being annoyed or
hindered by the movement system.

## Using LibGDX Input Handling

The class responsible for above-mentioned player actions is the `PlayerController`. It extends the `InputProcessor`
class provided by LibGDX to handle input events. The `PlayerController` listens for key presses and releases, updating
the player's state accordingly.
See [Class
`PlayerController`](https://under-the-oaks.github.io/ColdCase-Client/tech/underoaks/coldcase/game/PlayerController.html)
for more
details on the implementation.