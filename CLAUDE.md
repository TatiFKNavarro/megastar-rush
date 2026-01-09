# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Running the Game

Open `index.html` directly in a web browser. No build step, dependencies, or server required.

## Architecture

Single-file HTML5 canvas game with embedded CSS and JavaScript. The entire application is in `index.html`.

### Class Structure

- **Player** - Character state, position, jumping physics, "YEAH!" text animation
- **Obstacle** - Individual obstacle entity with position and movement
- **ObstacleManager** - Spawning, updates, and progressive difficulty scaling
- **ScoreManager** - Time-based scoring (1 point per second)
- **CollisionDetector** - Static AABB collision detection utility
- **Renderer** - All canvas drawing (ground, player, obstacles, UI, start screen)
- **InputManager** - Event listener management with pub/sub pattern for spacebar input
- **Game** - Main orchestrator tying all systems together via game loop

### Key Patterns

- `CONFIG` object at top of script centralizes all game parameters
- Each class has `reset()` for game restart, `update()` for frame logic, `getBounds()` for collision rectangles
- Game loop uses `requestAnimationFrame` calling `update()` then `render()`
- Canvas context passed through constructors for dependency injection

### Game Flow

1. `Game` constructor initializes all managers and player
2. Start screen shown via `showStartScreen()`
3. Spacebar triggers `start()` which begins game loop
4. Each frame: update player physics → update obstacles → check collisions → update score → render
5. Collision triggers `gameOver()` showing overlay with restart button
