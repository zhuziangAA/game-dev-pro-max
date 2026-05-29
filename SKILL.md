---
name: game-dev-pro-max
description: "Use when you need to guide a complete beginner through developing a game from zero — choosing scope, prototyping, building core loop, polishing, and publishing. Works for browser games (Phaser/Canvas), mobile games, and desktop games. Covers the full pipeline with concrete templates and anti-patterns."
version: 1.0.0
author: 夏娃 Hermes Agent
license: MIT
metadata:
  hermes:
    tags: [game, gamedev, beginner, tutorial, phaser, full-cycle, indie-dev]
    related_skills: [writing-plans, systematic-debugging, subagent-driven-development, architecture-diagram, excalidraw, test-driven-development]
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

## 🧠 Game Design Principles (from 100 Principles of Game Design)

This skill is built on the foundation of *100 Principles of Game Design* (Despain et al., 2013). Below are the principles most relevant to building your first game, distilled into actionable advice. The full reference (90+ principles with complete text) is in `references/principles_full_content.md`.

### Chapter 1: Innovation — 游戏创新

| Principle | 核心含义 | 新手怎么做 |
|---|---|---|
| **Core Gameplay Loop (已覆盖)** | 游戏就是玩家反复做的一件事 | Phase 2 已经实现 |
| **Flow** | 挑战与技能的动态平衡。太难=焦虑，太简单=无聊 | 用难度曲线保持"刚好有点难但能过" |
| **MDA Framework** | Mechanics(机制) → Dynamics(行为) → Aesthetics(感受) | 设计机制时想: "这会让玩家有什么感受?" |
| **Magic Circle** | 游戏创造了一个与现实隔离的"魔法圈" | 用主题和规则让玩家沉浸 |
| **Bartle's Player Types** | 玩家分4类: 成就者/探索者/社交者/杀手 | 确保你的游戏至少服务2类玩家 |
| **Fairness** | 玩家感知的公平决定留存 | 随机必须有约束，避免明显的"不公平" |
| **Feedback Loops** | 正反馈(赢家通吃) vs 负反馈(给落后补偿) | 赛车游戏用"后车加速"是负反馈，很好 |
| **Lazzaro's Four Keys** | 4种乐趣: Easy Fun(好奇), Hard Fun(挑战), People Fun(社交), Serious Fun(意义) | 知道你的游戏主打哪种乐趣 |
| **Koster's Theory of Fun** | 乐趣=大脑学习模式时的愉悦感 | 让玩家不断学新东西，别一直重复 |
| **Skinner Box** | 不可预测的奖励最让人上瘾 | 不要滥用！用随机掉落做惊喜，别做"精神控制" |
| **Transparency** | 玩家需要理解游戏为什么这样运作 | 死亡要告诉玩家"为什么死" |

### Chapter 2: Creation — 游戏制作

| Principle | 核心含义 | 新手怎么做 |
|---|---|---|
| **Scope (范围控制)** | 砍掉80%的功能才能做完20%的好游戏 | Phase 0 已经覆盖 |
| **Paper Prototyping** | 在纸上测试游戏逻辑，比写代码快10倍 | 棋盘游戏用纸片测试规则，跳过写代码 |
| **Play Testing** | 看着别人玩，不说话 | Phase 5 已经覆盖 |
| **Iteration (迭代)** | 做一版→试玩→改进→再试 | Phase 5 已经覆盖 |
| **Game Pillars** | 用3个关键词定义游戏的核心 | 例如: "探索、战斗、升级" |
| **Flow (见上)** | 难度曲线就是心流曲线 | 每关比前一关难10-15% |
| **Design by Committee** | 太多人参与决策会毁掉游戏 | 小游戏1个设计师就够了 |
| **User-Centered Design** | 从玩家角度设计，不是从开发者角度 | 新手教程不是"教怎么玩"，是"让玩家自然学会" |
| **Define the Problem** | 先搞清楚你要解决的核心问题 | "我想做一个2D跳跃游戏" ❌ \n"我想让玩家体验在危险悬崖间跳跃的紧张感" ✅ |
| **The 80/20 Rule** | 20%的功能产生80%的乐趣 | 先做那20% |
| **Ooh, Shiny!** | 视觉/音效刺激抓住注意力 | 但不要过度依赖，游戏性才是根本 |
| **Theme (主题)** | 一致的主题让游戏有灵魂 | 像素风就全像素，卡通就全卡通 |
| **Time and Money** | 时间vs金钱的二选一 | 初学者选择"花时间不花钱"（用免费素材） |
| **Pick Two: Fast, Cheap, Good** | 三选二，必有一项牺牲 | 🆓新手策略: 快+便宜(不追求完美) |
| **Objects, Attributes, States** | 万物皆可建模为: 对象→属性→状态 | 写代码前用这个框架梳理游戏实体 |
| **Prototyping (通用)** | 快速试错，而不是完美规划 | Phase 1 的 HTML 模板就是你的原型工具 |
| **Metagames** | 游戏之外的元游戏（社区、攻略、排行榜） | 先别做，等游戏做好了再考虑 |

### Chapter 3: Balancing — 游戏平衡

| Principle | 核心含义 | 新手怎么做 |
|---|---|---|
| **Balancing and Tuning** | 平衡不是一次完成，是反复调整 | 用数据驱动的调整: 记录死玩家次数，调参数 |
| **Doubling and Halving** | 调数值时翻倍或减半，别微调 | 敌人血量太高? 减半试。玩家太强? 翻倍试 |
| **Loss Aversion** | 人害怕失去远大于渴望获得 | 死亡惩罚要适度，防止玩家气馁放弃 |
| **Variable Rewards** | 随机奖励比固定奖励更让人着迷 | 宝箱开出的东西不完全一样 |
| **Learning Curve** | 从简单到复杂的递进学习 | Level 1只有移动, Level 2加跳跃, Level 3加敌人 |
| **Interest Curve** | 玩家的兴趣随时间波动 | 游戏应该有"高潮-低谷-高潮"的节奏 |
| **Errors Players Make** | 玩家会犯错, 但好的设计让错误可理解 | 玩家落地前显示落点范围 |
| **Errors Without Punishment** | 有些错误不应该惩罚 | 死一次重生比"从头再来"好 |
| **Punishment (惩罚)** | 惩罚要重到有意义，轻到不劝退 | 新手模式降低惩罚强度 |
| **Details (细节)** | 细节决定品质感 | 跳起来时衣服飘动、踩到泥地有脚印 |
| **Addiction Pathways** | 游戏成瘾的道德问题 | 设计时要考虑玩家的健康 |
| **Attention vs. Perception** | 人注意到的≠实际看到的 | 用视觉引导玩家注意关键信息 |
| **Hick's Law** | 选择越多，决策越慢 | 新手教程一次只给1-2个按钮 |
| **Ten Minutes of Sustained Attention** | 人的持续注意力约10分钟 | 每个游戏环节控制在10分钟内 |
| **Maslow's Hierarchy** | 先满足基本需求，再追求高级快乐 | 游戏也要从"生存"→"社交"→"自我实现" |
| **Economies of Scale** | 批量生产更便宜 | 用同一个敌人贴图变色做3种敌人 |

### Chapter 4: Troubleshooting — 问题排查

| Principle | 核心含义 | 新手怎么做 |
|---|---|---|
| **Cognitive Biases** | 开发者无法客观评价自己的游戏 | 朋友说"还不错"≠真的不错。看行为，别听言语 |
| **Fundamental Attribution Error** | 玩家觉得"游戏没做好"而不是"我不会玩" | 玩家卡关时，先想是不是游戏设计有问题 |
| **Affordance Cues** | 物体的形态暗示其功能 | 可攀爬的墙要有把手/藤蔓，可开的门要有把手 |
| **Fitts' Law** | 目标越大越近，点击越快 | 手机游戏的按钮要大！ |
| **Dominant Strategy** | 如果有一个永远最优的策略，游戏就死了 | 石头剪刀布没有"最优"——让它循环克制 |
| **Griefing** | 总有人恶意破坏其他玩家的体验 | 多人游戏要防恶意行为（踢人、刷屏） |
| **Satisficing vs. Optimizing** | 大多数人"够好就行"而不是"追求最优" | 不需要做出史上最好的游戏，做个有趣的就行 |
| **Sense of Accomplishment** | 让玩家感觉自己变强了 | 每过一关给个小奖励/称号 |
| **Golden Ratio** | 1:1.618的视觉比例让人觉得美 | UI布局时参考这个比例 |
| **Krug's First Law** | "不要让我思考" —— 界面要一目了然 | 按钮放在玩家期待的位置 |
| **Zero-Sum Game** | 你赢=我输，制造对抗性 | PvP游戏天生零和，PvE需要设计合作 |
| **Pacing (节奏)** | 张弛有度，不要让玩家一直紧张 | 紧张关卡后跟一个轻松关 |
| **Working Memory** | 人一次只能记住7±2件事 | 别让玩家同时追踪太多东西 |
| **Instant vs. Delayed Gratification** | 即时反馈(吃豆) vs 延迟奖励(攒钱买装备) | 两种都要有 |
| **Music and Dopamine** | 好音乐能激发多巴胺 | 免费音乐推荐: incompetech.com |
| **Time Dilation** | 紧张时感觉时间变慢 | 致命时刻用慢动作增加戏剧性 |
| **Spatial Awareness** | 玩家需要对空间有感知 | 2D游戏确保前景/背景/可交互元素清晰区分 |
| **Advance Organizers** | 先给框架再给细节 | 教程先展示"整个游戏长啥样"再教具体操作 |

---

## 📚 Reference Files

This skill includes these reference files in `references/`:

- **principles_full_content.md** (90 principles, 6,500+ lines) — Full extracted text of every principle from *100 Principles of Game Design*. Load with skill_view(name='game-dev-pro-max', file_path='references/principles_full_content.md') when you need the complete explanation of any principle.
- **100-principles-of-game-design.md** — A-Z index + chapter structure + mapping to Game Dev Pro Max phases.

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
