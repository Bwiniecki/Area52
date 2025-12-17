# Area52 - 3D Space Invaders Game

**GROUP ID: 0323**

**GAME LINK:  https://bwiniecki.github.io/Area52/ **

A modern 3D remake of the classic Space Invaders arcade game built with Three.js. Battle waves of increasingly difficult aliens in immersive 3D space while dodging attacks and using special abilities.

---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Game Modes](#game-modes)
- [Controls](#controls)
- [Gameplay Mechanics](#gameplay-mechanics)
  - [Wave System](#wave-system)
  - [Alien Types](#alien-types)
  - [Special Abilities](#special-abilities)
  - [Scoring System](#scoring-system)
- [Technical Details](#technical-details)
  - [Technologies Used](#technologies-used)
  - [Project Structure](#project-structure)
  - [Game Configuration](#game-configuration)
- [Visual Effects](#visual-effects)
- [Installation & Setup](#installation--setup)
- [How to Play](#how-to-play)
- [Game Design](#game-design)

---

## Overview

Area52 is a 3D space shooter that reimagines the classic Space Invaders gameplay with modern graphics, smooth animations, and progressive difficulty. Players control a spaceship defending against waves of alien invaders that advance closer with each passing wave.

---

## Features

### Core Gameplay
- **3D Graphics**: Built using Three.js for immersive 3D rendering
- **Progressive Difficulty**: Waves become increasingly challenging with more aliens
- **Multiple Alien Types**: Face different enemy types with unique behaviors
- **Special Laser Ability**: Powerful screen-clearing weapon available every 5 waves
- **Lives System**: Start with 5 lives and survive as long as possible
- **Score Tracking**: Earn 100 points per alien defeated
- **Particle Effects**: Visual feedback for hits and explosions

### Visual Features
- **Dynamic Starfield**: 2000+ animated background stars
- **Shooting Stars**: Ambient meteor effects for atmosphere
- **Particle System**: Hit indicators and explosion effects
- **Responsive UI**: Clean HUD showing score, wave, lives, and laser status
- **Mobile Support**: Touch controls for mobile devices

---

## Game Modes

### Prototype Rendering Mode

Area52 features a **Prototype Mode** toggle that allows you to switch between detailed 3D models and simple primitive geometries during gameplay. This feature is perfect for:
- Performance optimization on lower-end devices
- Testing and development
- Retro/minimalist aesthetic preference
- Reduced visual complexity

#### How It Works

**Accessing Prototype Mode:**
- Click the **"PROTOTYPE"** button in the top-right corner during gameplay
- The button is only active once the game has started
- Toggle can be activated at any point during a wave

**Visual Changes:**

When Prototype Mode is **ENABLED** (button shows "FULL MODE"):
- **Player Ship**: Blue square (flat plane geometry)
- **Normal Aliens**: Green circles (flat circular geometry)
- **Charger Aliens**: Orange ovals (ellipsoid/stretched sphere geometry)
- **All objects**: No textures, shadows disabled for better performance
- **Bullets**: Remain unchanged (orange spheres)

When Prototype Mode is **DISABLED** (button shows "PROTOTYPE"):
- **Player Ship**: Detailed spaceship model with wings, cockpit, and engines
- **Normal Aliens**: Detailed green alien models with tentacles and eyes
- **Charger Aliens**: Detailed red/purple alien models with armor and teeth
- **All objects**: Full 3D models with shadows and materials

#### Persistent State
- The rendering mode persists across waves
- New aliens spawning in subsequent waves will match the current rendering mode
- Toggle back and forth at any time to switch between modes
- Button text updates to show which mode you can switch TO:
  - Shows "FULL MODE" when in prototype (click to return to full models)
  - Shows "PROTOTYPE" when in full mode (click to activate prototype)

**Note**: The game initially loads in Full Mode by default, using detailed 3D models if available.

---

## Controls

### Desktop Controls
| Key | Action |
|-----|--------|
| **Arrow Left** / **A** | Move ship left |
| **Arrow Right** / **D** | Move ship right |
| **Spacebar** | Shoot bullets |
| **Q** | Activate laser (when available) |

---

## Gameplay Mechanics

### Wave System

Each wave increases in difficulty:

1. **Base Aliens**: Start with 12 aliens in Wave 1
2. **Progressive Scaling**: Each wave adds 1 additional alien
3. **Grid Formation**: Aliens spawn in a grid pattern with randomized positions for variety
4. **Advancement**: Aliens continuously move toward the player at 0.054 units per frame
5. **Wave Transition**: Brief pause between waves with announcement

**Formula**: `Alien Count = 12 + (Wave - 1)`

### Alien Types

#### Normal Aliens
- **Health**: 1 hit to destroy
- **Behavior**: Move steadily toward player
- **Starting Wave**: Wave 1
- **Grid Layout**: 
  - Arranged in rows and columns
  - Spacing: 4 units (X-axis), 5 units (Z-axis)
  - Random offset: ±1.5 units for irregular appearance
- **Spawn Position**: Z = -25 (back of play area)
- **Points**: 100 per alien

#### Charger Aliens
- **Health**: 2 hits to destroy
- **Behavior**: Aggressively charge toward player at 4x normal speed
- **Starting Wave**: Wave 3
- **Scaling**: 
  - Wave 3-5: 1 charger
  - Wave 6-8: 2 chargers
  - Wave 9+: 3+ chargers (increases every 3 waves)
- **Formula**: `Charger Count = floor((Wave - 3) / 3) + 1`
- **Spawn Position**: Z = -33 or farther (behind normal aliens)
- **Visual**: Red coloration (prototype mode) or distinct model (full mode)
- **Points**: 100 per charger

### Special Abilities

#### Laser System
- **Availability**: Every 5 waves (waves 5, 10, 15, 20, etc.)
- **Activation**: Press **L** key (desktop) or laser button (mobile)
- **Duration**: 1.5 seconds
- **Effect**: 
  - Destroys ALL aliens on screen instantly
  - Full-screen energy beam visual effect
  - Screen shake for dramatic impact
- **Cooldown**: Cannot be used again until next 5-wave interval
- **Visual Indicator**: Glowing cyan "LASER READY!" text when available

### Scoring System

- **Points per Alien**: 100 (both normal and charger)
- **Display**: Top-left corner, cyan color
- **Persistence**: Score accumulates across waves until game over

### Lives System

- **Starting Lives**: 5
- **Life Loss**: Occurs when an alien reaches the player (Z > 3)
- **Game Over**: When lives reach 0

---

## Technical Details

### Project Structure

```
Area52/
├── index.html          # Main HTML file with UI structure
├── gp_main.js          # Core game logic and rendering
├── models/             # 3D model assets (GLB files)
│   ├── ship.glb
│   └── Alien.glb
└── README.md           # This file
```

### Game Configuration

Key parameters defined in `CONFIG` object:

#### Player Settings
- **Speed**: 0.22 units/frame
- **Bounds**: ±13 units (X-axis movement limit)
- **Position**: Z = 5 (fixed depth)

#### Bullet Settings
- **Speed**: 0.6 units/frame
- **Range**: 60 units (Z-axis travel distance)
- **Cooldown**: 238ms between shots
- **Radius**: 0.2 units
- **Color**: Orange (#ff6600)

#### Alien Settings
- **Base Count**: 12 aliens
- **Spacing**: 4 units (X), 5 units (Z)
- **Speed**: 0.054 units/frame (normal)
- **Charger Speed**: 0.216 units/frame (4× normal)
- **Start Position**: Z = -25
- **Random Offset**: ±1.5 units
- **Hit Detection**: 1.8 unit radius
- **Player Hit Distance**: 1.5 units

#### Visual Settings
- **Star Count**: 2000 stars
- **Starfield Radius**: 100 units
- **Camera Position**: (0, 8, 12)
- **Camera FOV**: 60 degrees

---

## Visual Effects

### Particle Systems

#### Hit Particles
- Spawn when aliens are destroyed
- 8 particles per hit
- Explode outward in random directions
- Orange color with glow effect
- Fade out over 0.3 seconds

#### Bullet Trails
- Subtle glow effect
- Point light attached to each bullet
- Dynamic shadows

### Starfield Animation
- **Background Stars**: 2000 white point stars
- **Slow Motion**: Parallax scrolling effect
- **Random Sizes**: Varied star magnitudes

### Shooting Stars
- **Frequency**: Spawn every 2-5 seconds
- **Trajectory**: Diagonal paths across scene
- **Trail Effect**: Line geometry following star
- **Colors**: Yellow-orange glow
- **Lifetime**: Auto-removes when off-screen

### Screen Effects
- **Wave Announcements**: Fade-in/out transitions
- **Laser Activation**: Full-screen beam + camera shake
- **Game Over Screen**: Red glow with shadow effects
- **UI Pulsing**: Laser availability indicator

---

## Setup

### Prerequisites
- Modern web browser with WebGL support

### Quick Start

1. Go to game link (https://bwiniecki.github.io/Area52/)

2. **Start Playing**:
   Click the "START GAME" button

---

## How to Play

### Objective
Survive as many waves as possible by destroying aliens before they reach your position.

### Basic Strategy

1. **Movement**: Stay mobile to avoid trapped positions
2. **Positioning**: Keep aliens centered in your field of fire
3. **Prioritization**: Target closer aliens first
4. **Chargers**: Focus on red charger aliens - they're faster and tougher
5. **Laser Usage**: Save laser for overwhelming waves or emergency situations
6. **Spacing**: Use full width of play area to manage alien spread

### Advanced Tips

- **Bullet Conservation**: Maintain steady fire rate to maximize hits
- **Wave Planning**: Prepare for next wave during transition screens
- **Charger Management**: Eliminate chargers early before they build up
- **Laser Timing**: Best used on waves 10, 15, 20 for maximum impact

### Win Conditions
There is no final wave - the game continues indefinitely with increasing difficulty. The goal is to achieve the highest score possible!

### Lose Conditions
- All 5 lives are depleted
- Aliens reach the player's Z-position (Z > 3)

---

## Game Design

### Design Philosophy
Area52 balances nostalgia with modern game design:
- **Accessibility**: Easy to learn, difficult to master
- **Progressive Challenge**: Smooth difficulty curve
- **Visual Feedback**: Clear indicators for all actions
- **Responsive Controls**: Immediate player input response
- **Performance**: Optimized for smooth 60 FPS gameplay

### Technical Implementation

#### Rendering Loop
- **60 FPS Target**: Consistent frame timing
- **Update Cycle**: 
  1. Process input
  2. Update player position
  3. Update bullets
  4. Update aliens
  5. Check collisions
  6. Update particles
  7. Update shooting stars
  8. Render scene

#### Collision Detection
- **Bounding Box System**: Three.js Box3 for accurate hit detection
- **Bullet-Alien**: Sphere-box intersection testing
- **Alien-Player**: Distance-based detection (1.5 unit threshold)
- **Optimization**: Only check active, alive entities

#### Memory Management
- **Object Pooling**: Reuse geometries where possible
- **Cleanup**: Remove off-screen entities
- **Material Cloning**: Independent materials per alien for hit effects

### Performance Optimizations
- Limit particle count per hit
- Remove bullets beyond play area (-60 units)
- Efficient array iteration (reverse loops for removals)
- Lazy loading of 3D models
- Fallback to primitives if models unavailable

---

## Credits

**Developed by**: CS559 Fall 2025 - Group 0323

**Technologies**: Three.js, WebGL, JavaScript ES6

**Inspired by**: Classic Space Invaders (1978) by Tomohiro Nishikado

---

This project is created for educational purposes as part of CS559 coursework.

---

**Enjoy the game and see how many waves you can survive!**
