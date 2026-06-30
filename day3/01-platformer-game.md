# Build a Platformer Game 🎮

## 🎯 Objective
Build a complete side-scrolling platformer game with a player character, platforms, coins, enemies, and 3 levels. Use arrow keys to move and spacebar to jump!

## 📚 What You'll Learn
- Game loops and frame-by-frame animation
- Physics (gravity, velocity, acceleration)
- Collision detection (are two rectangles overlapping?)
- Level design with arrays
- State management (menus, playing, game over)
- Keyboard input handling

## 🖥️ How to Run This
**Option A:** Create files in vscode.dev and use Live Preview
**Option B:** Paste all the code into [CodePen.io](https://codepen.io)

---

## 📖 Concepts You'll Use (Read This First!)

### 1. The Game Loop — The Heartbeat of Every Game
Every game works the same way: a function that runs ~60 times per second. Each "frame" it:
1. Reads input (what keys are pressed?)
2. Updates game state (move player, apply gravity, check collisions)
3. Draws everything on screen
```javascript
function gameLoop() {
    handleInput();    // Check keyboard
    update();         // Move things, apply physics
    draw();           // Render everything
    requestAnimationFrame(gameLoop);  // Do it again next frame!
}
```

### 2. Gravity & Velocity — Fake Physics
Gravity is just adding a small number to the player's vertical speed every frame:
```javascript
player.velocityY += 0.5;   // Gravity pulls down (increase speed)
player.y += player.velocityY;  // Apply speed to position

// When player lands on a platform:
player.velocityY = 0;  // Stop falling
player.y = platform.y - player.height;  // Sit on top
```

### 3. Collision Detection — Are Two Boxes Touching?
Everything in our game is a rectangle. Two rectangles overlap if:
```javascript
function isColliding(a, b) {
    return a.x < b.x + b.width &&
           a.x + a.width > b.x &&
           a.y < b.y + b.height &&
           a.y + a.height > b.y;
}
// If ALL four conditions are true = they're overlapping!
```

### 4. Keyboard Tracking — Smooth Movement
Instead of moving on key press, we track which keys are currently held down:
```javascript
let keys = {};

document.addEventListener("keydown", function(e) {
    keys[e.code] = true;   // Key is being held
});

document.addEventListener("keyup", function(e) {
    keys[e.code] = false;  // Key was released
});

// In the game loop:
if (keys["ArrowRight"]) {
    player.x += 5;  // Move right while key is held
}
```

### 5. Level Data — Designing Levels with Arrays
Each level is just a list of objects describing where things are:
```javascript
const level1 = {
    platforms: [
        { x: 0, y: 550, width: 800, height: 50 },    // Ground
        { x: 200, y: 400, width: 100, height: 20 },   // Floating platform
    ],
    coins: [
        { x: 230, y: 370 },
        { x: 500, y: 300 },
    ],
    enemies: [
        { x: 400, y: 520, speed: 2 }
    ]
};
```

### How These Concepts Connect:
| Concept | Role in the Game |
|---------|-----------------|
| Game Loop | Runs everything 60 times/second |
| Gravity & Velocity | Makes the player fall and jump realistically |
| Collision Detection | Checks if player hits platforms, coins, or enemies |
| Keyboard Tracking | Smooth left/right movement and jumping |
| Level Data | Defines where platforms, coins, and enemies are placed |

Now let's build! 👇

---

## 🛠️ Step-by-Step: The Complete Game

### Create `index.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Platformer Game</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="game-wrapper">
        <!-- Start Screen -->
        <div id="start-screen" class="overlay">
            <h1>🎮 Pixel Runner</h1>
            <p>Collect all coins. Avoid enemies. Reach the flag!</p>
            <div class="controls-info">
                <span>← → Move</span>
                <span>Space / ↑ Jump</span>
            </div>
            <button id="start-btn" class="game-btn">Start Game</button>
        </div>

        <!-- Game Over Screen -->
        <div id="gameover-screen" class="overlay hidden">
            <h1>💀 Game Over!</h1>
            <p>Score: <span id="final-score">0</span></p>
            <button id="retry-btn" class="game-btn">Try Again</button>
        </div>

        <!-- Win Screen -->
        <div id="win-screen" class="overlay hidden">
            <h1>🏆 You Win!</h1>
            <p>Final Score: <span id="win-score">0</span></p>
            <p class="sub-text">You completed all 3 levels!</p>
            <button id="replay-btn" class="game-btn">Play Again</button>
        </div>

        <!-- HUD -->
        <div id="hud" class="hud hidden">
            <span id="hud-level">Level: 1</span>
            <span id="hud-score">Score: 0</span>
            <span id="hud-lives">❤️❤️❤️</span>
        </div>

        <!-- Game Canvas -->
        <canvas id="game-canvas" width="800" height="600"></canvas>
    </div>

    <script src="script.js"></script>
</body>
</html>
```

### Create `style.css`:

```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    background: #1a1a2e;
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

.game-wrapper {
    position: relative;
    border-radius: 12px;
    overflow: hidden;
    box-shadow: 0 20px 60px rgba(0, 0, 0, 0.5);
}

#game-canvas {
    display: block;
    background: #87CEEB;
}

/* Overlays (Start, Game Over, Win screens) */
.overlay {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.85);
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    z-index: 10;
    color: white;
    gap: 15px;
}

.overlay.hidden {
    display: none;
}

.overlay h1 {
    font-size: 3rem;
    text-shadow: 0 0 20px rgba(255, 255, 255, 0.3);
}

.overlay p {
    font-size: 1.1rem;
    opacity: 0.8;
}

.sub-text {
    font-size: 0.9rem !important;
    opacity: 0.6 !important;
}

.controls-info {
    display: flex;
    gap: 20px;
    background: rgba(255, 255, 255, 0.1);
    padding: 10px 20px;
    border-radius: 8px;
    font-size: 0.9rem;
}

.game-btn {
    margin-top: 10px;
    padding: 14px 35px;
    font-size: 1.1rem;
    font-weight: 600;
    border: none;
    border-radius: 10px;
    cursor: pointer;
    background: linear-gradient(45deg, #4ecdc4, #44bd6e);
    color: white;
    transition: all 0.2s ease;
}

.game-btn:hover {
    transform: translateY(-3px);
    box-shadow: 0 8px 20px rgba(78, 205, 196, 0.4);
}

/* HUD */
.hud {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    display: flex;
    justify-content: space-between;
    padding: 12px 20px;
    background: rgba(0, 0, 0, 0.5);
    color: white;
    font-weight: 600;
    font-size: 0.95rem;
    z-index: 5;
}

.hud.hidden {
    display: none;
}
```

### Create `script.js`:

```javascript
// ===========================
// SETUP
// ===========================
const canvas = document.getElementById("game-canvas");
const ctx = canvas.getContext("2d");

// Screen elements
const startScreen = document.getElementById("start-screen");
const gameoverScreen = document.getElementById("gameover-screen");
const winScreen = document.getElementById("win-screen");
const hud = document.getElementById("hud");
const hudLevel = document.getElementById("hud-level");
const hudScore = document.getElementById("hud-score");
const hudLives = document.getElementById("hud-lives");

// Game state
let gameState = "start"; // start, playing, gameover, win
let score = 0;
let lives = 3;
let currentLevel = 0;

// ===========================
// KEYBOARD INPUT
// ===========================
let keys = {};

document.addEventListener("keydown", function (e) {
    keys[e.code] = true;
    // Prevent page scrolling with arrow keys/space
    if (["ArrowUp", "ArrowDown", "ArrowLeft", "ArrowRight", "Space"].includes(e.code)) {
        e.preventDefault();
    }
});

document.addEventListener("keyup", function (e) {
    keys[e.code] = false;
});

// ===========================
// PLAYER
// ===========================
let player = {
    x: 50,
    y: 400,
    width: 30,
    height: 40,
    velocityX: 0,
    velocityY: 0,
    speed: 4,
    jumpPower: -12,
    onGround: false,
    color: "#ff6b6b",
    direction: 1  // 1 = right, -1 = left
};

// ===========================
// PHYSICS CONSTANTS
// ===========================
const GRAVITY = 0.6;
const FRICTION = 0.8;
const MAX_FALL_SPEED = 15;

// ===========================
// LEVEL DATA
// ===========================
const levels = [
    // LEVEL 1 - Easy introduction
    {
        platforms: [
            { x: 0, y: 560, width: 800, height: 40 },       // Ground
            { x: 150, y: 440, width: 120, height: 15 },
            { x: 350, y: 360, width: 120, height: 15 },
            { x: 550, y: 280, width: 120, height: 15 },
            { x: 650, y: 440, width: 120, height: 15 }
        ],
        coins: [
            { x: 190, y: 410, collected: false },
            { x: 390, y: 330, collected: false },
            { x: 590, y: 250, collected: false },
            { x: 690, y: 410, collected: false },
            { x: 400, y: 530, collected: false }
        ],
        enemies: [
            { x: 300, y: 530, width: 30, height: 30, speed: 1.5, minX: 200, maxX: 500 }
        ],
        flag: { x: 740, y: 230, width: 20, height: 50 },
        playerStart: { x: 50, y: 500 },
        bgColor: "#87CEEB"
    },
    // LEVEL 2 - More platforms, more enemies
    {
        platforms: [
            { x: 0, y: 560, width: 200, height: 40 },       // Left ground
            { x: 250, y: 480, width: 80, height: 15 },
            { x: 380, y: 400, width: 80, height: 15 },
            { x: 500, y: 320, width: 100, height: 15 },
            { x: 350, y: 240, width: 80, height: 15 },
            { x: 150, y: 300, width: 100, height: 15 },
            { x: 600, y: 480, width: 200, height: 15 },
            { x: 650, y: 200, width: 100, height: 15 }
        ],
        coins: [
            { x: 270, y: 450, collected: false },
            { x: 400, y: 370, collected: false },
            { x: 530, y: 290, collected: false },
            { x: 370, y: 210, collected: false },
            { x: 180, y: 270, collected: false },
            { x: 680, y: 450, collected: false },
            { x: 700, y: 170, collected: false }
        ],
        enemies: [
            { x: 600, y: 450, width: 30, height: 30, speed: 2, minX: 600, maxX: 780 },
            { x: 100, y: 530, width: 30, height: 30, speed: 1.5, minX: 0, maxX: 180 }
        ],
        flag: { x: 700, y: 150, width: 20, height: 50 },
        playerStart: { x: 50, y: 500 },
        bgColor: "#2c3e50"
    },
    // LEVEL 3 - Hard! Tight jumps, more enemies
    {
        platforms: [
            { x: 0, y: 560, width: 100, height: 40 },
            { x: 150, y: 500, width: 60, height: 15 },
            { x: 270, y: 440, width: 60, height: 15 },
            { x: 380, y: 380, width: 60, height: 15 },
            { x: 280, y: 300, width: 60, height: 15 },
            { x: 150, y: 240, width: 60, height: 15 },
            { x: 300, y: 180, width: 80, height: 15 },
            { x: 480, y: 300, width: 80, height: 15 },
            { x: 600, y: 220, width: 80, height: 15 },
            { x: 500, y: 500, width: 100, height: 15 },
            { x: 700, y: 140, width: 80, height: 15 }
        ],
        coins: [
            { x: 170, y: 470, collected: false },
            { x: 290, y: 410, collected: false },
            { x: 400, y: 350, collected: false },
            { x: 300, y: 270, collected: false },
            { x: 170, y: 210, collected: false },
            { x: 330, y: 150, collected: false },
            { x: 510, y: 270, collected: false },
            { x: 630, y: 190, collected: false },
            { x: 730, y: 110, collected: false }
        ],
        enemies: [
            { x: 500, y: 470, width: 30, height: 30, speed: 2.5, minX: 500, maxX: 580 },
            { x: 480, y: 270, width: 30, height: 30, speed: 2, minX: 480, maxX: 540 },
            { x: 600, y: 190, width: 30, height: 30, speed: 1.5, minX: 600, maxX: 660 }
        ],
        flag: { x: 730, y: 90, width: 20, height: 50 },
        playerStart: { x: 30, y: 500 },
        bgColor: "#1a1a2e"
    }
];

// Active level data (will be reset each level)
let platforms = [];
let coins = [];
let enemies = [];
let flag = {};

// ===========================
// LOAD A LEVEL
// ===========================
function loadLevel(levelIndex) {
    const level = levels[levelIndex];

    // Deep copy level data so we can modify it
    platforms = JSON.parse(JSON.stringify(level.platforms));
    coins = JSON.parse(JSON.stringify(level.coins));
    enemies = JSON.parse(JSON.stringify(level.enemies));
    flag = JSON.parse(JSON.stringify(level.flag));

    // Reset player position
    player.x = level.playerStart.x;
    player.y = level.playerStart.y;
    player.velocityX = 0;
    player.velocityY = 0;

    // Update HUD
    hudLevel.textContent = "Level: " + (levelIndex + 1);
    updateHUD();
}

// ===========================
// COLLISION DETECTION
// ===========================
function isColliding(a, b) {
    return a.x < b.x + b.width &&
           a.x + a.width > b.x &&
           a.y < b.y + b.height &&
           a.y + a.height > b.y;
}

// ===========================
// UPDATE PLAYER
// ===========================
function updatePlayer() {
    // Horizontal movement
    if (keys["ArrowLeft"]) {
        player.velocityX = -player.speed;
        player.direction = -1;
    } else if (keys["ArrowRight"]) {
        player.velocityX = player.speed;
        player.direction = 1;
    } else {
        player.velocityX *= FRICTION;
    }

    // Jumping
    if ((keys["Space"] || keys["ArrowUp"]) && player.onGround) {
        player.velocityY = player.jumpPower;
        player.onGround = false;
    }

    // Apply gravity
    player.velocityY += GRAVITY;
    if (player.velocityY > MAX_FALL_SPEED) {
        player.velocityY = MAX_FALL_SPEED;
    }

    // Move player
    player.x += player.velocityX;
    player.y += player.velocityY;

    // Keep player in bounds (horizontal)
    if (player.x < 0) player.x = 0;
    if (player.x + player.width > canvas.width) player.x = canvas.width - player.width;

    // Fall off screen = lose a life
    if (player.y > canvas.height + 50) {
        loseLife();
        return;
    }

    // Platform collision
    player.onGround = false;
    for (let i = 0; i < platforms.length; i++) {
        const plat = platforms[i];
        if (isColliding(player, plat)) {
            // Landing on top of platform
            if (player.velocityY > 0 && player.y + player.height - player.velocityY <= plat.y + 5) {
                player.y = plat.y - player.height;
                player.velocityY = 0;
                player.onGround = true;
            }
            // Hitting bottom of platform
            else if (player.velocityY < 0 && player.y - player.velocityY >= plat.y + plat.height - 5) {
                player.y = plat.y + plat.height;
                player.velocityY = 0;
            }
        }
    }
}

// ===========================
// UPDATE ENEMIES
// ===========================
function updateEnemies() {
    for (let i = 0; i < enemies.length; i++) {
        const enemy = enemies[i];

        // Move enemy back and forth
        enemy.x += enemy.speed;
        if (enemy.x <= enemy.minX || enemy.x + enemy.width >= enemy.maxX) {
            enemy.speed = -enemy.speed;
        }

        // Check collision with player
        if (isColliding(player, enemy)) {
            loseLife();
            return;
        }
    }
}

// ===========================
// UPDATE COINS
// ===========================
function updateCoins() {
    for (let i = 0; i < coins.length; i++) {
        const coin = coins[i];
        if (coin.collected) continue;

        const coinBox = { x: coin.x, y: coin.y, width: 20, height: 20 };
        if (isColliding(player, coinBox)) {
            coin.collected = true;
            score += 10;
            updateHUD();
        }
    }
}

// ===========================
// CHECK FLAG (Level Complete)
// ===========================
function checkFlag() {
    if (isColliding(player, flag)) {
        currentLevel++;
        if (currentLevel >= levels.length) {
            // Won the game!
            gameState = "win";
            document.getElementById("win-score").textContent = score;
            winScreen.classList.remove("hidden");
            hud.classList.add("hidden");
        } else {
            // Next level
            loadLevel(currentLevel);
        }
    }
}

// ===========================
// LOSE A LIFE
// ===========================
function loseLife() {
    lives--;
    updateHUD();

    if (lives <= 0) {
        gameState = "gameover";
        document.getElementById("final-score").textContent = score;
        gameoverScreen.classList.remove("hidden");
        hud.classList.add("hidden");
    } else {
        // Respawn at start of current level
        const level = levels[currentLevel];
        player.x = level.playerStart.x;
        player.y = level.playerStart.y;
        player.velocityX = 0;
        player.velocityY = 0;
    }
}

// ===========================
// UPDATE HUD
// ===========================
function updateHUD() {
    hudScore.textContent = "Score: " + score;
    let heartsText = "";
    for (let i = 0; i < lives; i++) {
        heartsText += "❤️";
    }
    hudLives.textContent = heartsText;
}

// ===========================
// DRAWING
// ===========================
function draw() {
    const level = levels[currentLevel];

    // Background
    ctx.fillStyle = level.bgColor;
    ctx.fillRect(0, 0, canvas.width, canvas.height);

    // Draw some background clouds/stars depending on level
    drawBackground(level.bgColor);

    // Draw platforms
    for (let i = 0; i < platforms.length; i++) {
        const plat = platforms[i];
        ctx.fillStyle = "#4a3728";
        ctx.fillRect(plat.x, plat.y, plat.width, plat.height);
        // Grass top
        ctx.fillStyle = "#6bcb77";
        ctx.fillRect(plat.x, plat.y, plat.width, 5);
    }

    // Draw coins
    for (let i = 0; i < coins.length; i++) {
        const coin = coins[i];
        if (coin.collected) continue;

        ctx.beginPath();
        ctx.arc(coin.x + 10, coin.y + 10, 10, 0, Math.PI * 2);
        ctx.fillStyle = "#ffd700";
        ctx.fill();
        ctx.strokeStyle = "#daa520";
        ctx.lineWidth = 2;
        ctx.stroke();

        // Coin shine
        ctx.beginPath();
        ctx.arc(coin.x + 7, coin.y + 7, 3, 0, Math.PI * 2);
        ctx.fillStyle = "rgba(255, 255, 255, 0.6)";
        ctx.fill();
    }

    // Draw enemies
    for (let i = 0; i < enemies.length; i++) {
        const enemy = enemies[i];
        // Body
        ctx.fillStyle = "#e74c3c";
        ctx.fillRect(enemy.x, enemy.y, enemy.width, enemy.height);
        // Eyes
        ctx.fillStyle = "white";
        ctx.fillRect(enemy.x + 5, enemy.y + 8, 8, 8);
        ctx.fillRect(enemy.x + 17, enemy.y + 8, 8, 8);
        // Pupils
        ctx.fillStyle = "black";
        ctx.fillRect(enemy.x + 8, enemy.y + 11, 4, 4);
        ctx.fillRect(enemy.x + 20, enemy.y + 11, 4, 4);
        // Spikes on top
        ctx.fillStyle = "#c0392b";
        for (let s = 0; s < 3; s++) {
            ctx.beginPath();
            ctx.moveTo(enemy.x + 5 + s * 10, enemy.y);
            ctx.lineTo(enemy.x + 10 + s * 10, enemy.y - 8);
            ctx.lineTo(enemy.x + 15 + s * 10, enemy.y);
            ctx.fill();
        }
    }

    // Draw flag
    ctx.fillStyle = "#8B4513";
    ctx.fillRect(flag.x + 8, flag.y, 4, flag.height);
    // Flag cloth
    ctx.fillStyle = "#27ae60";
    ctx.beginPath();
    ctx.moveTo(flag.x + 12, flag.y);
    ctx.lineTo(flag.x + 35, flag.y + 12);
    ctx.lineTo(flag.x + 12, flag.y + 24);
    ctx.fill();

    // Draw player
    drawPlayer();
}

function drawPlayer() {
    const p = player;

    // Body
    ctx.fillStyle = p.color;
    ctx.fillRect(p.x, p.y, p.width, p.height);

    // Face
    ctx.fillStyle = "#ffd5a0";
    ctx.fillRect(p.x + 5, p.y + 3, 20, 15);

    // Eyes
    ctx.fillStyle = "white";
    ctx.fillRect(p.x + 8, p.y + 7, 6, 6);
    ctx.fillRect(p.x + 17, p.y + 7, 6, 6);

    // Pupils (look in direction of movement)
    ctx.fillStyle = "black";
    const pupilOffset = player.direction === 1 ? 2 : 0;
    ctx.fillRect(p.x + 9 + pupilOffset, p.y + 9, 3, 3);
    ctx.fillRect(p.x + 18 + pupilOffset, p.y + 9, 3, 3);

    // Feet (animate when moving)
    ctx.fillStyle = "#333";
    if (Math.abs(player.velocityX) > 0.5 && player.onGround) {
        const step = Math.floor(Date.now() / 150) % 2;
        if (step === 0) {
            ctx.fillRect(p.x + 3, p.y + p.height - 6, 10, 6);
            ctx.fillRect(p.x + 17, p.y + p.height - 3, 10, 3);
        } else {
            ctx.fillRect(p.x + 3, p.y + p.height - 3, 10, 3);
            ctx.fillRect(p.x + 17, p.y + p.height - 6, 10, 6);
        }
    } else {
        ctx.fillRect(p.x + 3, p.y + p.height - 5, 10, 5);
        ctx.fillRect(p.x + 17, p.y + p.height - 5, 10, 5);
    }
}

function drawBackground(bgColor) {
    if (bgColor === "#87CEEB") {
        // Level 1: clouds
        ctx.fillStyle = "rgba(255, 255, 255, 0.6)";
        ctx.beginPath(); ctx.arc(100, 80, 30, 0, Math.PI * 2); ctx.fill();
        ctx.beginPath(); ctx.arc(130, 80, 40, 0, Math.PI * 2); ctx.fill();
        ctx.beginPath(); ctx.arc(160, 80, 30, 0, Math.PI * 2); ctx.fill();
        ctx.beginPath(); ctx.arc(500, 120, 25, 0, Math.PI * 2); ctx.fill();
        ctx.beginPath(); ctx.arc(530, 120, 35, 0, Math.PI * 2); ctx.fill();
        ctx.beginPath(); ctx.arc(560, 120, 25, 0, Math.PI * 2); ctx.fill();
    } else {
        // Level 2 & 3: stars
        ctx.fillStyle = "rgba(255, 255, 255, 0.5)";
        const starPositions = [
            [50, 30], [150, 60], [300, 20], [450, 50], [600, 30],
            [700, 70], [250, 90], [550, 80], [100, 100], [400, 110]
        ];
        for (let i = 0; i < starPositions.length; i++) {
            ctx.beginPath();
            ctx.arc(starPositions[i][0], starPositions[i][1], 2, 0, Math.PI * 2);
            ctx.fill();
        }
    }
}

// ===========================
// GAME LOOP
// ===========================
function gameLoop() {
    if (gameState === "playing") {
        updatePlayer();
        updateEnemies();
        updateCoins();
        checkFlag();
        draw();
    }
    requestAnimationFrame(gameLoop);
}

// ===========================
// START / RESTART GAME
// ===========================
function startGame() {
    gameState = "playing";
    score = 0;
    lives = 3;
    currentLevel = 0;

    startScreen.classList.add("hidden");
    gameoverScreen.classList.add("hidden");
    winScreen.classList.add("hidden");
    hud.classList.remove("hidden");

    loadLevel(currentLevel);
}

// ===========================
// BUTTON EVENTS
// ===========================
document.getElementById("start-btn").addEventListener("click", startGame);
document.getElementById("retry-btn").addEventListener("click", startGame);
document.getElementById("replay-btn").addEventListener("click", startGame);

// ===========================
// START THE GAME LOOP
// ===========================
gameLoop();
```

---

## 🎮 How the Game Works (Summary)

```
┌─────────────────────────────────────────────┐
│  GAME LOOP (runs 60 times per second)       │
│                                             │
│  1. Read keyboard input                     │
│  2. Move player (apply velocity)            │
│  3. Apply gravity (pull player down)        │
│  4. Check platform collisions (stop fall)   │
│  5. Move enemies (back and forth)           │
│  6. Check coin collection (add score)       │
│  7. Check enemy collision (lose life)       │
│  8. Check flag collision (next level)       │
│  9. Draw everything                         │
│                                             │
│  → Repeat forever!                          │
└─────────────────────────────────────────────┘
```

---

## 💡 Understanding Each Part

### Gravity:
```
Frame 1: velocityY = 0    (standing still)
Frame 2: velocityY = 0.6  (starting to fall)
Frame 3: velocityY = 1.2  (falling faster)
Frame 4: velocityY = 1.8  (faster!)
Frame 5: velocityY = 2.4  (accelerating!)
→ Hit platform: velocityY = 0 (stopped!)
```
Gravity makes falling feel natural because it accelerates.

### Jumping:
```
Jump pressed: velocityY = -12 (launch upward!)
Frame by frame: -12, -11.4, -10.8, ... -0.6, 0, 0.6, 1.2...
→ Goes up, slows down, stops, falls back down
```
Just like real life!

### Collision Detection:
```
Player: x=100, y=200, width=30, height=40
Platform: x=80, y=240, width=120, height=15

Are they overlapping? Check all 4 sides:
- Player left (100) < Platform right (200)? YES ✓
- Player right (130) > Platform left (80)? YES ✓
- Player top (200) < Platform bottom (255)? YES ✓
- Player bottom (240) > Platform top (240)? YES ✓
All true = COLLISION!
```

---

## 🎨 Creative Challenges

### Challenge 1: Add Double Jump
Let the player jump once more while in the air:
```javascript
let jumpCount = 0;
const MAX_JUMPS = 2;

// In the jump code:
if ((keys["Space"] || keys["ArrowUp"]) && jumpCount < MAX_JUMPS) {
    player.velocityY = player.jumpPower;
    jumpCount++;
}
// Reset when landing:
if (player.onGround) jumpCount = 0;
```

### Challenge 2: Add Moving Platforms
```javascript
// In level data, add:
{ x: 300, y: 350, width: 80, height: 15, moving: true, speed: 1, minX: 250, maxX: 450 }

// In update, move them:
platforms.forEach(function(plat) {
    if (plat.moving) {
        plat.x += plat.speed;
        if (plat.x <= plat.minX || plat.x + plat.width >= plat.maxX) {
            plat.speed = -plat.speed;
        }
    }
});
```

### Challenge 3: Add Sound Effects
```javascript
// Create sounds (no file needed - use Web Audio API)
function playJumpSound() {
    const audio = new AudioContext();
    const osc = audio.createOscillator();
    osc.frequency.setValueAtTime(400, audio.currentTime);
    osc.frequency.exponentialRampToValueAtTime(800, audio.currentTime + 0.1);
    osc.connect(audio.destination);
    osc.start(); osc.stop(audio.currentTime + 0.1);
}
```

### Challenge 4: Power-Ups
Add a speed boost or invincibility:
```javascript
const powerups = [
    { x: 350, y: 300, type: "speed", collected: false }
];

// When collected:
if (powerup.type === "speed") {
    player.speed = 7;  // Faster!
    setTimeout(function() { player.speed = 4; }, 5000);  // Back to normal after 5s
}
```

### Challenge 5: Design Your Own Levels!
Copy a level object and change the numbers. Tips:
- Canvas is 800 wide, 600 tall
- `y: 0` is the TOP, `y: 600` is the BOTTOM
- Platforms need to be reachable by jumping (max jump height ≈ 130px)
- Place coins slightly above platforms so players need to jump

### Challenge 6: Add a Timer
```javascript
let levelStartTime = Date.now();

// In HUD update:
let elapsed = Math.floor((Date.now() - levelStartTime) / 1000);
hudLevel.textContent = "Level: " + (currentLevel + 1) + " | Time: " + elapsed + "s";

// Bonus points for fast completion!
```

---

## 🔍 What Did You Learn?
- Game loop architecture (the backbone of ALL games)
- Physics simulation (gravity, velocity, friction)
- Collision detection (axis-aligned bounding boxes)
- Level design and data structures
- State management (start, playing, game over, win)
- Keyboard input handling
- Canvas 2D drawing
- Game balancing (platform spacing, enemy speed)

## 🎓 Real-World Connection
These are the same concepts used in:
- **Unity / Unreal Engine** — same physics and collision systems
- **Scratch** — same game loop, just visual
- **Professional indie games** — Celeste, Hollow Knight, Super Meat Boy all started with these exact mechanics
- **Mobile games** — Geometry Dash, Crossy Road, Flappy Bird

You've built something real. Be proud of it! 🏆
