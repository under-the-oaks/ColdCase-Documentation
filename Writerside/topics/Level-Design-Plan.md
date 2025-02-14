# Level Design Plan

## Overview

This document defines the design principles, structure, and expected behavior of the tutorial levels in *ColdCase*. It
outlines how players will be introduced to core mechanics and how the levels will gradually increase in complexity to
foster cooperative gameplay.

It also describes the difficulty of creating levels based on important questions and how we went about answering them.

---

# Tutorial Levels

## Design Goals - Tutorial

1. **Progressive Learning Curve:** Each level should introduce a new mechanic, reinforcing previously learned concepts.
2. **Encouraging Cooperation:** Players must learn to communicate and work together to solve puzzles.
3. **Ensuring Clarity:** Clear visual cues and intuitive interactions to minimize confusion.

---

## Level Structure

### **Level 1 – The First Step**

- **Objective:** Teach players how local blocks work.
- **Mechanics Introduced:**
    - Players move **local blocks** that do not affect the parallel world.
    - Learn that **each world has separate physics**.
- **Expected Outcome:**
    - Players understand how their actions affect only their world.
    - They start experimenting with movement mechanics.
      ![Level1Detective.jpeg](Level1Detective.jpeg) ![ghost1_pic.png](ghost1_pic.png)

---

### **Level 2 – The Detective’s Glove**

- **Objective:** Introduce the glove and portal mechanics.
- **Mechanics Introduced:**
    - The **glove** allows interaction with objects.
    - The **portal** is used to transfer the glove between players.
- **Expected Outcome:**
    - Players recognize that passing the glove is necessary for solving puzzles.
    - They begin to coordinate actions to progress.
      ![Level2Detective.jpeg](Level2Detective.jpeg) ![Level2Ghost.jpeg](Level2Ghost.jpeg)

---

### **Level 3 – The Switch**

- **Objective:** Introduce the concept of switches and their effect on spikes.
- **Mechanics Introduced:**
    - The **switch** can be activated using the glove.
    - Activating a switch removes spikes **in the parallel world**.
- **Expected Outcome:**
    - Players understand that they need to collaborate to clear obstacles.
    - They develop a strategy for interacting with shared game elements.
      ![Level3Detective.jpeg](Level3Detective.jpeg) ![Level3Ghost.jpeg](Level3Ghost.jpeg)

---

## Conclusion - Tutorial

The level design ensures a **smooth learning curve**, reinforcing cooperative gameplay while introducing mechanics
progressively. Through **gradual complexity increases and clear guidance**, players will be well-prepared for the full
game experience.

---

# Main Level

## Design Goals - Main Level

1. **Challenging the player:** Players should be challenged (especially in later levels), but not overwhelmed.
2. **Encourage team play:** The levels should be structured in such a way that they encourage team play and require good
   communication.
3. **Fun:** In addition to the other aspects, the levels are primarily intended to be fun for the players.

---

## Important Questions

1. How easy/difficult is the level actually?
2. Is the level solvable?
3. Is the level “breakable”?
4. Is the level (still) fun?

---

## How could we answer these questions?

1. **Sketches:** Sketches have helped enormously in sketching levels before the actual implementation and in determining
   which objects should appear in the level and where they can be placed.
2. **Testing:** Testing the levels in particular was crucial to answering the questions above. Above all, it quickly
   became clear whether the level was fun and solvable. An important aspect of this was that the people involved had
   nothing to do with creating the levels. This provided a clear and unbiased view.

---

# Main Level Overview

### **Level 1 – Easy**

| ![level_leicht.png](level_leicht.png) | ![level_1Ghost.png](level_1Ghost_3.png) |
 |:--------------------------------------|----------------------------------------:|
| detective view                        |                              ghost view |

### **Level 2 – Medium**

| ![level_mittel.png](level_mittel.png) | ![level_2Ghost.png](level_2Ghost.png) |
|:--------------------------------------|--------------------------------------:|
| detective view                        |                            ghost view |

### **Level 3 – Hard**

| ![level_schwer.png](level_schwer.png) | ![level_3Ghost.png](level_3Ghost.png) |
|:--------------------------------------|--------------------------------------:|
| detective view                        |                            ghost view |

## Conclusion - Main Level

Creating complete levels is an important and challenging task. After all, they represent the actual game. A level must
always be solvable, unbreakable and fun at the same time.

In our particular case, these points must also apply to two players at the same time. The more difficult a level is to
be, the more aspects have to be taken into consideration when implementing it.