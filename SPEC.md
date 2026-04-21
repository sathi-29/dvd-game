# DVD Bouncer - Roguelike Game Prototype Specification

## Project Overview

- **Project Name**: DVD Bouncer
- **Project Type**: 2D Roguelike Browser Game
- **Core Functionality**: A bouncing "DVD logo" square bounces around the screen. Players place tiles on borders to trigger score multipliers and special effects when the logo collides with tiles.
- **Target Users**: Casual gamers looking for an addictive idle/clicker-style roguelike experience

---

## UI/UX Specification

### Layout Structure

**Main Game Screen (Center)**
- Canvas: 800x600 pixels
- Border area: 40px wide clickable zones around all four edges
- Game area (playable): 720x520 pixels (inside border)

**HUD (Top)**
- Score display: Top-left
- Gold display: Top-right
- Round indicator: Top-center
- Target score: Below round indicator

**Shop Panel (Right Side - 280px width)**
- Tile selection buttons
- Current gold
- Owned tiles count

**Control Panel (Bottom)**
- Start/Stop button
- Clear tiles button
- Next Round button (appears when target reached)

### Visual Design

**Color Palette**
- Background: `#0a0a0f` (deep navy-black)
- Border inactive: `#1a1a2e` (dark purple-gray)
- Border hover: `#2d2d44` (lighter purple-gray)
- DVD Logo: `#ff3366` (hot pink/magenta)
- Primary accent: `#00ffaa` (neon cyan-green)
- Secondary accent: `#ffaa00` (gold/amber)
- Text primary: `#e0e0e0` (light gray)
- Text secondary: `#888888` (medium gray)
- Danger/Explosive: `#ff4444` (red)
- Multiplier: `#44aaff` (blue)
- Generator: `#44ff44` (green)

**Typography**
- Primary font: "JetBrains Mono", monospace
- Headings: 18px bold
- Body text: 14px regular
- Score display: 24px bold
- Small labels: 12px

**Spacing System**
- Base unit: 8px
- Component padding: 16px
- Section gaps: 24px

**Visual Effects**
- DVD logo: Subtle glow effect (`box-shadow` or canvas glow)
- Tile placement: Pulse animation on drop
- Score gain: Float-up text animation (+points)
- Explosive: Screen shake + particle burst
- Multiplier active: Border glow color change

### Components

**DVD Logo (Bouncing Object)**
- Size: 30x30 pixels
- Color: Hot pink with subtle glow
- Bounce effect: Slight squash on wall hit
- Trail effect: Fading afterimage

**Tile Objects**
- Size: 40x40 pixels (occupies border zone)
- Types:
  1. **Multiplier Tile** (Blue): 2x score multiplier for 5 hits
  2. **Explosive Tile** (Red): Clears all tiles in 1-tile radius
  3. **Generator Tile** (Green): Generates 1 gold per 10 seconds
  4. **Slow Tile** (Yellow): Reduces DVD speed by 20%
  5. **Teleport Tile** (Purple): Teleports DVD to random position

**Border Zones**
- 4 clickable edges (top, bottom, left, right)
- Visual: Subtle grid pattern
- Hover state: Highlighted border section

**Shop Buttons**
- 80x60 pixels each
- Shows tile icon + name + price
- Disabled state: Grayed out when insufficient gold

**Score Display**
- Large numeric display
- Animated counting effect
- Gold icon next to gold amount

---

## Functionality Specification

### Core Features

**1. DVD Logo Physics**
- Initial spawn: Center of screen
- Initial velocity: Random direction, speed 3-5 pixels/frame
- Wall bounce: Reflects off screen edges (not border tiles)
- Speed increase: +0.5 per round

**2. Tile Placement System**
- Click border edge to place selected tile
- Tiles snap to grid on border (10 slots per edge)
- Max 40 tiles total (10 per edge)
- Tiles occupy border, not play area

**3. Collision Detection**
- Check DVD bounding box vs tile bounding box
- On collision:
  - Trigger tile effect
  - Add score (base: 10 points)
  - Apply multiplier if active
  - Visual feedback

**4. Score System**
- Base score per hit: 10 points
- Multiplier stacking: 2x per multiplier tile (max 4x)
- Score from multipliers: `10 * multiplier_count`
- Gold earned: `floor(score / 100)`

**5. Round Structure**
- Round 1 target: 500 points
- Target increase: `previous_target * 1.5`
- Time limit: None (untimed)
- Round complete: When target score reached
- Bonus gold on round complete: `round_number * 50`

**6. Shop System**
- Starting gold: 100
- Tile prices:
  - Multiplier: 50 gold
  - Explosive: 75 gold
  - Generator: 100 gold
  - Slow: 60 gold
  - Teleport: 80 gold

**7. Tile Effects**
- Multiplier: Lasts 5 hits, then degrades
- Explosive: One-time use, clears adjacent tiles
- Generator: Passive, generates gold over time
- Slow: Lasts 10 seconds
- Teleport: One-time use

### User Interactions

1. **Start Game**: Click "Start" to begin DVD movement
2. **Place Tile**: Select tile type from shop, click border edge
3. **Buy Tile**: Click shop button (deducts gold)
4. **Clear Tiles**: Click clear button (removes all tiles, no refund)
5. **Next Round**: Click when target reached

### Edge Cases

- DVD stuck in corner: Add small random velocity nudge
- No gold for purchase: Button disabled, show tooltip
- All border slots full: Cannot place more tiles
- Generator overflow: Max 500 gold stored

---

## Acceptance Criteria

### Visual Checkpoints

- [ ] Canvas renders at 800x600 with dark theme
- [ ] DVD logo appears as glowing pink square
- [ ] Border zones are visible and highlightable
- [ ] Tiles display with correct type colors
- [ ] Score and gold update in real-time
- [ ] Shop panel shows all 5 tile types with prices

### Functional Checkpoints

- [ ] DVD bounces off screen edges correctly
- [ ] Clicking border places selected tile
- [ ] Collision triggers score increase
- [ ] Multiplier tiles stack correctly
- [ ] Explosive clears adjacent tiles
- [ ] Generator adds gold over time
- [ ] Shop purchases deduct gold
- [ ] Round advances when target reached
- [ ] Game runs smoothly at 60fps

---

## Technical Implementation

- Single HTML file with embedded CSS and JavaScript
- HTML5 Canvas for game rendering
- RequestAnimationFrame for game loop
- No external dependencies (pure vanilla JS)