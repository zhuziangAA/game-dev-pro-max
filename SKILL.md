---
name: game-dev-pro-max
description: "Use when you need to guide a complete beginner through developing a game from zero — choosing scope, prototyping, building core loop, polishing, and publishing. Works for browser games (Phaser/Canvas), mobile games, and desktop games. Covers the full pipeline with concrete templates and anti-patterns."
version: 1.0.0
author: 夏娃 Hermes Agent
license: MIT
metadata:
  hermes:
    tags: [game, gamedev, beginner, tutorial, phaser, full-cycle, indie-dev]
    related_skills: [writing-plans, systematic-debugging, test-driven-development, subagent-driven-development, architecture-diagram, excalidraw]
---

# Game Dev Pro Max (从0开始开发一款游戏)

## Overview

This skill guides a **complete beginner** (no game development experience) through building a complete, playable game. It assumes basic programming ability (can read/write JavaScript/TypeScript or Python) but zero knowledge of game engines, game architecture, or the game development pipeline.

**Philosophy:** The fastest way to make a game is to **ship something playable first, polish later**. This skill focuses on getting you to a playable prototype as fast as possible, then iterating.

> **核心原则：** 先做一个能玩的版本，再谈好不好玩。完成比完美重要。

---

## Phase 0: Choose Your Game (选择你的第一个游戏)

### 🚨 Critical Decision: Your First Game

**Rule #1: Your FIRST game should be SMALL.**

Not "small for a game". **RIDICULOUSLY small.**

| Great First Game ✅ | Bad First Game ❌ |
|---|---|
| Snake / 贪吃蛇 | An MMORPG (大型多人在线) |
| Pong / 乒乓球 | An open-world RPG |
| Flappy Bird clone | A fighting game with netcode |
| Simple puzzle (Match-3 / 消消乐) | A card game with 200+ cards |
| 2D platformer with 3 levels | Your dream game |
| Breakout / 打砖块 | "Just a small RPG" (it never is) |

An experienced team of 5 people takes **6-12 months** to ship a "small" commercial game. A solo beginner should aim for **2-4 weeks** for the first playable version.

**Goal of the first game:** Finish it. Ship it. Learn what the full pipeline feels like.

### How to Pick Your Game

Ask these questions:

1. **What genre excites you?** (Platformer? Puzzle? Strategy? Card game?)
2. **What's the simplest version of that genre?** (Platformer → one level, no enemies; Strategy → Tic-Tac-Toe)
3. **Can you explain the core loop in one sentence?** (e.g., "Player moves left/right, jumps over holes, reaches the flag.")
4. **How many "things" does the player interact with?** (≤ 3 things → good start. 10+ things → too complex.)

If you can't explain the game to a friend in 30 seconds with a drawing on a napkin, it's too complex.

### Game Genres: Complexity Ranking (Simple → Complex)

```
Pong / Snake / Breakout            ← 1-3 days (JS/HTML canvas)
Flappy Bird / Infinite Runner      ← 3-5 days
Simple Puzzle (Match-3, Tetris)    ← 1-2 weeks
2D Platformer (one level)          ← 1-2 weeks
Turn-based Board Game (no AI)      ← 2-3 weeks
Card Game (simple rules)           ← 2-3 weeks
Tower Defense (basic)              ← 3-4 weeks
Top-down 2D RPG (1 town)           ← 4-8 weeks
Simple Multiplayer (2 player)      ← 4-8 weeks (adds server complexity)
```

---

## Phase 1: Setup & Scaffold (搭建开发环境)

### Tech Stack Decision

For a beginner, **pick ONE** of these paths:

| Path | Engine | Language | Output | Best For |
|---|---|---|---|---|
| **HTML Canvas (最简单)** | None, vanilla JS | JavaScript | Browser | Pong, Snake, Breakout, Flappy Bird |
| **Phaser 3 (推荐中级)** | Phaser 3 | JavaScript/TypeScript | Browser | Platformers, top-down, board games, puzzles |
| **Godot (推荐桌面)** | Godot 4 | GDScript / C# | Windows/Mac/Linux/Web | Anything 2D or 3D |
| **PICO-8 (趣味)** | PICO-8 fantasy console | Lua | Web/PNG cart | Retro games, game jams |

**Recommended for total beginners:** **HTML Canvas** first (no engine install, learn game core concepts), then **Phaser 3** for the real game.

### HTML Canvas Quick Start

```html
<!DOCTYPE html>
<html>
<head>
  <title>My Game</title>
  <style>
    body { margin: 0; background: #000; display: flex; justify-content: center; align-items: center; height: 100vh; }
    canvas { background: #fff; }
  </style>
</head>
<body>
  <canvas id="gameCanvas" width="800" height="600"></canvas>
  <script>
    const canvas = document.getElementById('gameCanvas');
    const ctx = canvas.getContext('2d');

    // === GAME LOOP ===
    let lastTime = 0;

    function gameLoop(timestamp) {
      const dt = (timestamp - lastTime) / 1000; // delta time in seconds
      lastTime = timestamp;

      update(dt);
      render();

      requestAnimationFrame(gameLoop);
    }

    // === UPDATE: game logic, physics, input ===
    function update(dt) {
      // TODO: move things, check collisions, update state
    }

    // === RENDER: draw everything ===
    function render() {
      ctx.clearRect(0, 0, 800, 600);

      // TODO: draw player, enemies, score, UI
      ctx.fillStyle = '#333';
      ctx.font = '24px Arial';
      ctx.fillText('Hello Game!', 300, 280);
    }

    // === INPUT ===
    const keys = {};
    document.addEventListener('keydown', (e) => keys[e.key] = true);
    document.addEventListener('keyup', (e) => keys[e.key] = false);

    // === START ===
    requestAnimationFrame(gameLoop);
  </script>
</body>
</html>
```

**Save as `index.html`** and open in browser. That's a working game shell in 50 lines.

### Phaser 3 Quick Start

```html
<!DOCTYPE html>
<html>
<head>
  <title>My Phaser Game</title>
  <script src="https://cdn.jsdelivr.net/npm/phaser@3.80.1/dist/phaser.min.js"></script>
  <style>
    body { margin: 0; background: #000; display: flex; justify-content: center; align-items: center; height: 100vh; }
  </style>
</head>
<body>
<script>
class MainScene extends Phaser.Scene {
  constructor() { super('MainScene'); }

  preload() {
    // this.load.image('player', 'assets/player.png');
  }

  create() {
    this.cursors = this.input.keyboard.createCursorKeys();
  }

  update(time, delta) {
    // if (this.cursors.left.isDown) this.player.x -= 5;
  }
}

const config = {
  type: Phaser.AUTO,
  width: 800,
  height: 600,
  scene: MainScene,
  backgroundColor: '#ffffff',
};

new Phaser.Game(config);
</script>
</body>
</html>
```

---

## Phase 2: Core Loop (核心玩法循环)

### What is a Game Loop?

Every game has a loop:

```
Input → Update (state, physics, AI) → Render (draw everything) → Repeat
```

The core gameplay loop is **the single sentence that describes what a player does repeatedly**. Examples:

| Game | Core Loop |
|---|---|
| Snake | Move head → eat food → grow → don't hit walls or self |
| Flappy Bird | Tap to flap → fly through pipes → don't hit ground/pipes → score |
| Tetris | Piece falls → rotate/move → complete lines → harder pieces come |
| Simple platformer | Move left/right → jump → reach flag → next level |

### Build the Core Loop First, Nothing Else

🚨 **Common beginner mistake:** Starting with menus, settings, splash screens, character artwork, backstory, music, or "polish."

**Rule #2: Your game doesn't exist until the core loop works.**

Build in this order:

```
1. SWITCH on a blank canvas  →  player can move
2. ADD interaction            →  player can affect something (jump, shoot, collect)
3. ADD challenge              →  something pushes back (enemies, timer, obstacles)
4. ADD win/lose condition     →  game knows when you've won or lost
5. ADD restart                →  player can try again
```

### Concrete Example: Building Pong (step by step)

**Step 1:** Draw two paddles and a ball (just rectangles)
**Step 2:** Move left paddle with W/S, right paddle with Arrow keys
**Step 3:** Ball bounces off top/bottom walls and paddles
**Step 4:** Ball goes past paddle → score for opponent
**Step 5:** First to 5 wins

That's a fully functional game in **~100 lines of JS**. No menus, no sound, no fancy graphics. **It's already playable.**

### Concrete Example: Building a 2D Platformer (step by step)

**Step 1:** Player rectangle moves left/right with arrow keys
**Step 2:** Add gravity and jumping (W or Space)
**Step 3:** Add a ground platform and one floating platform
**Step 4:** Add a **goal** (reach a flag/point at the end)
**Step 5:** Add a **failure** (fall into a pit)
**Step 6:** Add restart on failure

That's a playable game. **Everything else is extra.**

### Key Techniques for Core Loop

**Movement:**
```javascript
// Simple movement (top-down)
player.x += keys['ArrowRight'] - keys['ArrowLeft'] * speed * dt;
player.y += keys['ArrowDown'] - keys['ArrowUp'] * speed * dt;

// Platformer (with gravity)
player.vy += gravity * dt;
player.x += player.vx * dt;
player.y += player.vy * dt;
// Ground collision
if (player.y + player.height > groundY) { player.y = groundY - player.height; player.vy = 0; }
```

**Collision Detection (AABB):**
```javascript
function rectCollide(a, b) {
  return a.x < b.x + b.width && a.x + a.width > b.x &&
         a.y < b.y + b.height && a.y + a.height > b.y;
}
```

**State Machine (for game states):**
```javascript
const STATE = { MENU: 0, PLAYING: 1, GAME_OVER: 2, WIN: 3 };
let gameState = STATE.PLAYING;

function update(dt) {
  switch (gameState) {
    case STATE.PLAYING:
      updateGame(dt);
      break;
    case STATE.GAME_OVER:
      if (keys['Enter']) resetGame();
      break;
  }
}
```

---

## Phase 3: Add Content & Depth (丰富内容)

Once the core loop works, you can add:

### Level Design
- Add more levels (each level is just data — positions of platforms, enemies, goals)
- Level progression: each level slightly harder (more enemies, faster speed, tighter spaces)

### Score & Progression
- Score tracking, high scores
- Difficulty ramp (enemies get faster, more spawn, time limit decreases)
- Unlock simple visual rewards

### Simple AI
- Enemy moves toward player
- Enemy patrols between two points
- Enemy shoots at player periodically
- "Boss" that has more HP and a simple attack pattern

### Basic UI
- Score display (top corner)
- Health bar
- Lives counter
- Pause (Escape key → overlay)
- Simple instruction text

---

## Phase 4: Polish (打磨)

### When to Polish

**Rule #3:** Don't polish until the game is fun in its simplest form.

If the core game isn't fun with colored rectangles, adding better art won't fix it. Polish only when:
- The core loop is complete and playable
- You've had 3+ other people play it and said "the mechanics feel good"
- The game is winnable and losable with clear feedback

### Polish Layers

| Layer | What to Do | Time Estimate |
|---|---|---|
| **Sound** | SFX for actions (jump, hit, score, death), background music | 1-2 hours with free assets |
| **Visual** | Better colors, basic sprites, particles (explosions, dust) | 1-2 days |
| **Screen effects** | Screen shake on hit, flash on damage, smooth transitions | A few hours |
| **UI polish** | Smooth menus, animation, transitions between screens | 1-2 days |
| **Feel** | Juice! Bounce on landing, lerp/smooth movement, particle trails, camera shake | 2-3 days |

### Free Assets Resources

| Type | Source | URL |
|---|---|---|
| **Sound effects** | sfxr / jsfxr (generate your own) | https://sfxr.me |
| **Music** | Incompetech (Kevin MacLeod) | https://incompetech.com |
| **Sprites** | Kenney Assets | https://kenney.nl/assets |
| **Sprites** | itch.io free asset packs | https://itch.io/game-assets/free |
| **Sprites** | OpenGameArt | https://opengameart.org |
| **Fonts** | Google Fonts (use @font-face) | https://fonts.google.com |
| **Pixel art editor** | Piskel (browser-based) | https://www.piskelapp.com |

---

## Phase 5: Test & Iterate (测试迭代)

### Playtesting Protocol

1. **You play it** — fix bugs and obvious issues
2. **A friend plays it** — watch them, don't help. Note where they get confused.
3. **Fix confusion** — unclear instructions? Missing visual feedback? Wrong control mapping?
4. **Repeat** — each playtester finds ~5 issues. Fix them, test again.

### Common Beginner Pitfalls Discovered in Playtesting

| Symptom | Likely Cause | Fix |
|---|---|---|
| Player keeps dying without understanding why | No feedback on enemies/dangers | Add visual telegraph or sound |
| Player can't figure out controls | No instructions or bad controls | Show control hints, make WASD + Space standard |
| Game is too hard / too easy | Balancing off | Tweak speed, spawn rate, HP values |
| Player doesn't know if they're winning | No score/progress indicator | Add score display, level indicator |
| Player doesn't restart | No clear restart path | Add "Press R to restart" on death screen |

---

## Phase 6: Ship & Publish (发布)

### Publishing Options by Type

**Browser Game (easiest — free hosting):**
1. Export as HTML + JS (single file or folder)
2. Upload to **itch.io** (free, great game dev community)
3. Or upload to **GitHub Pages** (free, need basic git knowledge)

```bash
# If using Vite build
npm run build
# dist/ folder is your game — upload to itch.io or GitHub Pages
```

**itch.io Publishing:**
1. Create account at https://itch.io
2. Dashboard → Create new project
3. Upload the game folder as a ZIP
4. Set "This game will be played in a browser" (for HTML games)
5. Add screenshots, description, controls
6. Publish!

**Mobile (more complex):**
- Wrap HTML game with Cordova/Capacitor for iOS/Android
- Or use Godot export → APK/IPA directly

**Desktop:**
- Phaser game → wrap with Electron or Tauri
- Godot → built-in export to Windows/Mac/Linux

---

## Organizational Template

### Project Folder Structure (Recommended)

```
my-game/
├── index.html              # Entry point (for HTML games)
├── src/
│   ├── main.js             # Game config, scene registration
│   ├── scenes/
│   │   ├── BootScene.js    # Loading assets
│   │   ├── MenuScene.js    # Title screen
│   │   ├── GameScene.js    # Main gameplay
│   │   └── GameOverScene.js
│   ├── entities/
│   │   ├── Player.js       # Player class
│   │   └── Enemy.js        # Enemy class
│   ├── systems/
│   │   ├── Input.js        # Input handling
│   │   └── Collision.js    # Collision detection
│   └── config/
│       └── constants.js    # Game constants (speeds, sizes, etc.)
├── assets/
│   ├── images/             # Sprites, backgrounds
│   ├── audio/              # SFX, music
│   └── fonts/              # Custom fonts
└── README.md               # How to run
```

### Template: Game Design One-Pager

Before writing code, fill this out (keep it to one page):

```markdown
# Game: [Name]

## Core Loop (一句话)
[Player does X, then Y happens, player tries to Z]

## Controls
- Arrow Keys: Move
- Space: Jump / Action
- R: Restart

## Win Condition
[How do you win?]

## Lose Condition
[How do you lose?]

## Scoring
[What gives points?]

## Difficulty Curve
[Round 1: slow; Round 5: fast / Level 1: 3 enemies; Level 3: 10 enemies]

## Assets Needed (check if already available)
- [ ] Player sprite
- [ ] Enemy sprite
- [ ] Background
- [ ] Sound effects
- [ ] Music
```

---

## Common Pitfalls

1. **Scope creep (需求膨胀):** "I'll just add one more feature" is the #1 game killer. The game that ships will have 30% of the features you originally planned. **Plan for that.** Cut features ruthlessly. Every feature you add doubles the bug surface area.

2. **Starting with art/UI instead of core loop:** A beautiful menu with no game behind it is a screensaver. Build the game first, skin it later.

3. **Perfectionism (完美主义):** "The movement doesn't feel quite right yet, I'll fix it before adding enemies." No. Add enemies with the bad movement. Add the score with the ugly UI. Make it work, then make it good.

4. **No version control:** `git init` on day one. Commit after every working feature. You *will* break something and need to go back.

5. **Silent bugs:** If something doesn't work, the game doesn't show an error — it just doesn't do anything. Add `console.log` everywhere while building. Open the browser DevTools (F12) every time something seems off.

6. **Over-engineering:** "I need to architect this with ECS and dependency injection before I start." No. Start with a single file. When it gets too big (500+ lines), split it up. Premature architecture is wasted effort.

7. **Skipping the one-pager:** Writing down the core loop and win/lose conditions forces clarity. If you can't write it down, you can't code it.

8. **Testing only your own play style:** You know exactly what to do. Playtesters don't. Watch them play without helping. Every point of confusion is a bug in your UX.

9. **Trying to build multiplayer on the first game:** Multiplayer adds server architecture, networking, state sync, reconnection, anti-cheat — it easily 5x the complexity. Save it for game #3 or #4.

10. **Not shipping:** The game is never "done." Set a deadline. Ship on that date. Add a "v1.0" label and ship it. You can always make v1.1.

---

## Verification Checklist

- [ ] Game design one-pager is written (core loop, controls, win/lose, scoring)
- [ ] Tech stack chosen (HTML Canvas / Phaser / Godot)
- [ ] Core loop is playable (can start → play → win/lose → restart)
- [ ] At least 1 non-developer has played and given feedback
- [ ] Bugs found in playtesting are fixed
- [ ] Scope has been cut to a shippable size (not over-featured)
- [ ] Assets are either programmer art or from free sources
- [ ] Game is deployed on a platform (itch.io, GitHub Pages)
- [ ] README has controls and how-to-play instructions
- [ ] Version control initialized (git) and committed after each major phase
- [ ] Free asset sources bookmarked: Kenney, sfxr, OpenGameArt, itch.io free
