# Arcade-Project
Coordinate systems pygame.Vector2 Distance and magnitude Direction vectors Vector normalization Angles and rotation Sine and cosine Velocity Acceleration Momentum Drag/damping Dot products Field-of-view detection Simple AI decisions

# Unit 2 Game Design Document (GDD)
**Course:** Post-AP AI for Games
**Project:** Arcade Game (Vectors & Math AI)  
**Developer Name(s):** Julianne Peyton-Brene, Eddie Liao
**Date:** September 29th, 2026
---

## 1. Executive Summary & Core Loop

*   **Working Game Title:** Xtraterrost.io
*   **Arcade Theme / Style:** Top-down space Survival Battle 
*   **Elevator Pitch (1 Paragraph):** 
Xtraterrost.io is an endless space survival game. Move an astronaut using WASD controls while a laser automatically shoots the nearest alien. Defeat swarms and survive as long as possible. Every 30 seconds, aliens spawn faster, the laser fires faster, and its shots become wider.

*   **The Core Loop Diagram/Description:**
    [List the sequential loop of gameplay. Example: Drive vehicle -> Scan for checkpoints -> Dodge enemy drones -> Secure fuel -> Repeat]

Move and dodge -> Automatically shoot aliens -> Earn points -> Swarms and shots increase over time -> Repeat until health reaches zero (aka no health).
---

## 2. Game Objects, Mechanics & Controls

*   **Player-Controlled Object:** 
    *   *Entity Description:* Astronaut with 3 lives or (hp bar)
    *   *Input Controls:* WASD moves the player. A laser aims and fires automatically. Esc pauses; Enter starts or restarts.
*   **Required Game Object Classes:**
    1.  **Class 1 (Player):** `[Class Name: P1]` - Purpose: 1. Stores position, velocity, acceleration, health, and firing cooldown; handles movement and automatic firing.
    2.  **Class 2 (Enemy/Obstacle):** `[Class Name: Alien]` - Purpose: Alien stores position, velocity, facing, and AI state. All aliens have 20 health, move at 90 pixels/second, and damage the player on contact.

    3.  **Class 3 (Projectile/Collectible):** `[Class Name: LASER] - Purpose: Projectile stores position, velocity, damage, and width. Each shot deals 10 damage and disappears after hitting one alien or leaving the screen.
*   **Win/Loss Conditions & Objective:**
    *   *Primary Goal:* Survive and defeat as many aliens as possible; each kill gives 10 points. There is no final wave or victory screen. These increases happen automatically throughout the run. Spawn aliens along the arena edges, away from the player.
    *   *Failure State:* Alien contact removes 10 health. At zero health, show Game Over. Allow 0.75 seconds between damage events. Screen edges stop movement without damage.
    *   *UI Tracker Metrics:* Health, score, elapsed time, current spawn rate, firing rate, and shot width. The timer stops while paused; restarting resets all values. [List whether you are tracking Score, Health, Timer, Lives, etc.]

---

## 3. Unit 2 Math & AI Architecture

*   **Vector Movement & Kinematics (Reqs A & E):**
    *   *Positions & Velocities:* [Which entities will use `pygame.Vector2`?] Player, Alien, and Projectile use pygame.Vector2 for position and velocity. The player also stores acceleration. dt is elapsed frame time in seconds, capped at 0.05.
    *   *Acceleration & Drag Formula:* Player moves with drag, enemies do not. Similarly to the asteroid lab, players have a set velocity as they move through the game area, using WASD to change the player rect’s position per unit velocity. Acceleration happens on click when dashing, adding temporarily to the player’s velocity. [Explain how you will apply `velocity += acceleration`, `position += velocity`, and drag multipliers or maximum speed limits to your objects.]
*   **Distance & Normalization (Reqs B & C):**
    *   *Distance Math:* Enemies change from patrol to moving when the player is within a certain radius, calculate using .distance_to(). .distance_to() is also used for the vector along which bullets from the player are fired. [Where will you use `.distance_to()`? (e.g., Target range, collision, proximity sensors)] Use .distance_to() to find the nearest alien and check collisions. A hit occurs when distance <= the sum of collision radii. Projectile radius = shot_width / 2, so wider shots hit more easily. Remove a shot after its first hit.
    *   *Normalization Math:* Bullets function similarly to missiles. [Which object will calculate target directions via vector subtraction and `.normalize()`? What does it do with that vector?]
*   **Angles & Rotation (Req D):**
    *   *Orientation Method:* Enemies and players use trig to turn towards or away from each other similarly to the asteroid lab, however since the game does not exist in a boundless space, turning is limited to how players and enemies move left/up/right/down. [How will `math.cos()` and `math.sin()` be used to convert an angle into a forward movement vector? (e.g., player vehicle steering, rotating weapon turret)]
*   **AI Field-of-View & Dot Product (Req F & Part 3 AI):**
    *   **Perception:** Explained above. Player can sneak past/behind enemy’s field of view when the enemy is faced away. [What game world details does the computer object read? (e.g., tracking the distance vector to the player and the enemy's facing direction)]
    *   **Decision (The Dot Product Condition):** By default, an enemy is in patrol or idle mode. Once the player reaches the enemy’s front-facing field of view, it goes into attack mode. The field of view is a rectangular-based cone of vision in front of the enemy sprite. [Write out the conditional threshold logic using the dot product to establish an enemy's cone of sight.]
    *   **Action:** When a player is within around 25-50 in-game units of distance in front of the enmy, the enemy enters attack mode. [Describe what states change when the threshold condition is met. (e.g., `if dot > 0.8: state = ATTACK else: state = PATROL`)]

---

## 4. Visual & Interface Layout

*   **Primitive Shape Specifications:**
    *   *Player Shape/Color:* Shooter sprite [e.g., `pygame.draw.polygon()` as a green triangle]
    *   *Enemy Shape/Color:* Alien sprite (green rect as temp placeholder) [e.g., `pygame.draw.circle()` as a red sphere]
    *   *Projectile/Item Shape/Color:* yellow circle/bullet when fired [e.g., `pygame.draw.line()` as a yellow beam]
*   **Screen Layout & Text Elements:** font.render() screen.blit(): Health bottom left, score top left, elapsed time top middle or right, current spawn rate top/bottom right | else stored internally??? firing rate, and shot width.
    *   [Briefly explain where on the game window you will display the Title, Instructions, Current Score, and Health status indicators.]

---

## 5. Production Phases (Development Process)

*   **Phase 1: Minimum Playable Game (MVP)**
    *   [ ] Create basic Pygame window display and main loop.
    *   [ ] Instantiate basic player class with simple coordinate-changing keyboard controls.
    *   [ ] Render primitive enemy and projectile shapes.
    *   [ ] Hook up raw distance-based collision detection to trigger score/health updates.
*   **Phase 2: Unit 2 Math Integration**
    *   [ ] Transition movement architecture entirely to `pygame.Vector2`.
    *   [ ] Implement angular rotation, forward tracking vectors, acceleration forces, and velocity cap limits.
    *   [ ] Implement normalization equations for projectiles or chasing algorithms.
*   **Phase 3: AI Perception, Polish & UI Layout**
    *   [ ] Code the dot product mathematical view cone for your computer-controlled entity.
    *   [ ] Hook up conditional AI state swaps (e.g., Alerted vs. Passive states).
    *   [ ] Build start/end screens detailing game rules and final win/loss outcomes.

---

## 🤖 AI Attribution Note (If Applicable)
*   **LLM Engine Used:** [e.g., ChatGPT, GitHub Copilot, None]
*   **Assisted Sections:** [List exactly which sections or code structures the AI helped brainstorm or resolve.]
*   **Prompts Used:** 
    > "Example Prompt Used Here"
