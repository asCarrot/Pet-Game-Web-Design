# Wandering Ghost - Technical Specification

## 1. Technology Stack

To develop _Wandering Ghost_, we will use the following technology stack to support 2D graphics, animation, and gameplay mechanics:

### Frontend
- **JavaScript** - Main language for game logic.
- **Phaser.js** - 2D game engine for animations, physics, and keyboard controls.
- **HTML5 Canvas** - Rendering surface used by Phaser.

### Backend
- **Node.js** with **Express** - Backend for leaderboard management and data storage.
- **SQLite** - Database to store leaderboard data for simplicity.

### Additional Tools
- **Webpack** - For module bundling and dependency management.
- **REST API** - For managing player data and leaderboard functionality.

---

## 2. Architecture

The game architecture is modular, with separate components for managing the game state, map handling, player character, collectibles, enemies, timer, and leaderboard.

### Key Components

1. **GameManager**: Manages game state, level progression, score, and the gameplay loop.
2. **Map**: Handles room generation, entrances, exits, and secret rooms based on preset layouts.
3. **GhostCharacter**: Represents the player’s ghost with movement and interaction capabilities.
4. **Mob**: Represents enemy creatures with movement and collision detection.
5. **Collectible**: Manages spirit flames and power-ups.
6. **Timer**: Tracks the countdown for each level.
7. **Leaderboard**: Handles high scores and player progression.

---

## 3. Data Model

### 3.1 Class: `GameManager`

**Responsibilities**: Controls the game state, manages level progression, and tracks player score.

- **Attributes**:
  - `level: int` - The current level of the game.
  - `score: int` - The player’s score.
  - `isGameOver: boolean` - Indicates if the game is over.
  - `ghost: GhostCharacter` - Instance of the player-controlled ghost.
  - `mobs: List<Mob>` - List of mobs present in the level.
  - `collectibles: List<Collectible>` - List of spirit flames and power-ups.
  - `map: Map` - Instance of the `Map` class for room structure.
  - `timer: Timer` - Timer instance that manages the countdown.

- **Methods**:
  - `initializeGame()` - Sets up a new game with default score, level, and initializes game components.
  - `startLevel()` - Loads a specific preset layout for the level, resets ghost, mobs, and places collectibles.
  - `endLevel()` - Finalizes the level, increments `level`, and resets components for the next level.
  - `updateScore(points: int)` - Adds the specified points to `score`.
  - `checkGameOver(): boolean` - Checks if game-over conditions are met.
  - `toggleGamePause()` - Pauses or unpauses the game.
  - `tickGamePlay()` - Main gameplay loop, updating `ghost`, `mobs`, and `timer`.

---

### 3.2 Class: `Map`

**Responsibilities**: Handles room layouts based on preset configurations, with defined entrances, exits, and secret rooms.

- **Attributes**:
  - `layout: Array<Array<Tile>>` - 2D array representing the room’s layout (walls, pathways, entrances).
  - `entrance: Point` - Starting point for the ghost in the room.
  - `exit: Point` - The location of the room exit.
  - `secretRooms: List<Point>` - Coordinates of hidden entrances for secret rooms.

- **Methods**:
  - `loadPresetLayout(level: int)` - Loads a preset map layout for the given level from predefined layouts.
  - `checkCollision(position: Point): boolean` - Checks if the specified position collides with a wall.

---

### 3.3 Class: `GhostCharacter`

**Responsibilities**: Manages ghost movement, field of view, and interaction with collectibles.

- **Attributes**:
  - `position: Point` - Current position of the ghost within the room.
  - `fovRadius: int` - Radius of the ghost’s field of view.
  - `collisionRadius: int` - Radius for collision detection with mobs or collectibles.
  - `speed: int` - Movement speed of the ghost.

- **Methods**:
  - `move(direction: String)` - Moves the ghost in the specified direction (e.g., "up", "down").
  - `checkInteraction(): void` - Checks if the ghost is near a collectible or mob.
  - `renderFOV()` - Displays the ghost’s field of view visually on the map.

---

### 3.4 Class: `Mob`

**Responsibilities**: Represents an enemy creature with movement and collision detection.

- **Attributes**:
  - `position: Point` - Current position of the mob within the room.
  - `speed: int` - Movement speed of the mob.
  - `behaviorType: String` - AI type (e.g., "patrol", "chase").

- **Methods**:
  - `move(): void` - Updates the mob’s position based on its behavior.
  - `checkCollision(position: Point): boolean` - Determines if a collision occurs with the ghost.

---

### 3.5 Class: `Collectible`

**Responsibilities**: Represents spirit flames and power-ups.

- **Attributes**:
  - `position: Point` - Location of the collectible on the map.
  - `type: String` - Type of collectible (e.g., "spiritFlame", "powerUp").
  - `value: int` - Affection points or score value granted by the collectible.

- **Methods**:
  - `activate(): void` - Grants the player the collectible’s effect and removes it from the map.

---

### 3.6 Class: `Timer`

**Responsibilities**: Tracks the countdown for each level.

- **Attributes**:
  - `timeRemaining: int` - Time left in the current level (in seconds).

- **Methods**:
  - `startTimer(levelTime: int): void` - Initializes the timer with the specified level time.
  - `tick(): void` - Decrements the timer by one second.
  - `checkTimeUp(): boolean` - Returns `true` if the time runs out.

---

### 3.7 Class: `Leaderboard`

**Responsibilities**: Handles high scores and player progression.

- **Attributes**:
  - `entries: List<LeaderboardEntry>` - List of players and their scores.

- **Methods**:
  - `addEntry(playerName: String, score: int): void` - Adds a new score entry to the leaderboard.
  - `getTopScores(limit: int): List<LeaderboardEntry>` - Retrieves the top scores up to the specified limit.

---

## 4. Features

### 4.1 Core Features
- Ghost character navigation.
- Room exploration with procedurally designed layouts.
- Spirit flame collection and mob avoidance.
- Countdown timer for added challenge.

### 4.2 Additional Features
- Leaderboard tracking for high scores.
- Secret rooms with bonus collectibles.
- Unlockable levels with unique layouts.

---

## 5. Timeline

### Phase 1: Prototype (2 months)
- Develop basic mechanics for ghost movement and collectible interaction.

### Phase 2: Core Development (4 months)
- Implement mobs, room layouts, and the countdown timer.

### Phase 3: Polishing (2 months)
- Optimize UI, add animations, and bug fixes.

### Phase 4: Launch & Updates (Ongoing)
- Release new features, levels, and gather user feedback.

---

## 6. Inspiration
- **Duck Life**: Minigames and progression mechanics.
- **Pokémon XY Bottom Screen**: Engaging character interactions.
- **Tamagotchi**: Care-based mechanics and simplicity.
