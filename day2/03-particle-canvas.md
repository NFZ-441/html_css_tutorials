# Tutorial 3: Interactive Particle Canvas ✨

## 🎯 Objective
Create a mesmerizing particle animation where colorful dots follow your mouse, connect with lines, and create beautiful patterns. This is the kind of effect you see on modern websites!

## 📚 What You'll Learn
- HTML Canvas element (drawing with code!)
- Animation loops (requestAnimationFrame)
- Math for movement (position, speed, distance)
- Mouse tracking
- Object-oriented thinking (particle objects)

## 🖥️ How to Run This
**Option A:** Create files in vscode.dev and use Live Preview
**Option B:** Paste all the code into [CodePen.io](https://codepen.io)

---

## 📖 Concepts You'll Use (Read This First!)

### 1. HTML Canvas — Your Digital Drawing Paper
Canvas is a special HTML element that gives you a blank space to draw on with JavaScript. Unlike normal HTML where you place elements, canvas lets you draw shapes pixel by pixel.
```html
<canvas id="my-canvas"></canvas>
```
```javascript
const canvas = document.getElementById("my-canvas");
const ctx = canvas.getContext("2d");  // Get the "drawing tools"

// Now you can draw!
ctx.fillStyle = "red";
ctx.fillRect(10, 10, 50, 50);  // Draw a red square at x:10, y:10, size 50x50
```

### 2. Animation Loops — Making Things Move
To animate, you redraw the canvas ~60 times per second. Each frame you: clear → update positions → draw again.
```javascript
function animate() {
    // 1. Clear (or fade) the canvas
    // 2. Move objects
    // 3. Draw objects in new positions
    // 4. Request next frame
    requestAnimationFrame(animate);  // "Call me again next frame!"
}
animate();  // Start the loop
```

### 3. Objects — Grouping Related Data
An object bundles related values together. Each particle needs x, y, speed, color, etc:
```javascript
let particle = {
    x: 100,         // horizontal position
    y: 200,         // vertical position
    speedX: 2,      // how fast it moves sideways
    speedY: -1,     // how fast it moves up/down
    color: "red"
};

// Access values with dot notation:
particle.x = particle.x + particle.speedX;  // Move it!
```

### 4. Math.random() — Adding Randomness
Random numbers make each particle unique:
```javascript
Math.random()           // Random decimal between 0 and 1 (e.g., 0.73)
Math.random() * 100     // Random decimal between 0 and 100
Math.floor(Math.random() * 100)  // Random whole number 0-99
```

### 5. Distance Formula — Connecting Nearby Particles
To check if two particles are close enough to draw a line between them:
```javascript
let dx = particle1.x - particle2.x;  // horizontal gap
let dy = particle1.y - particle2.y;  // vertical gap
let distance = Math.sqrt(dx * dx + dy * dy);  // actual distance

if (distance < 120) {
    // They're close! Draw a line between them
}
```

### How These Concepts Connect in This Project:
| Concept | How We Use It |
|---------|---------------|
| Canvas | The full-screen area where particles are drawn |
| Animation Loop | Redraws all particles 60 times/second for smooth motion |
| Objects | Each particle stores its own position, speed, size, and color |
| Math.random() | Gives each particle a random starting position and speed |
| Distance Formula | Draws faint lines between particles that are near each other |

Now let's build! 👇

---

## 🛠️ Step-by-Step Instructions

### Step 1: Create the HTML Structure
Create `index.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Particle Canvas</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <!-- Controls Panel -->
    <div class="controls-panel">
        <h2>✨ Particle Canvas</h2>

        <div class="control">
            <label for="particle-count">Particles: <span id="count-display">80</span></label>
            <input type="range" id="particle-count" min="20" max="200" value="80">
        </div>

        <div class="control">
            <label for="particle-speed">Speed: <span id="speed-display">2</span></label>
            <input type="range" id="particle-speed" min="1" max="10" value="2">
        </div>

        <div class="control">
            <label for="particle-size">Size: <span id="size-display">3</span></label>
            <input type="range" id="particle-size" min="1" max="10" value="3">
        </div>

        <div class="control">
            <label for="connect-distance">Line Distance: <span id="distance-display">120</span></label>
            <input type="range" id="connect-distance" min="50" max="250" value="120">
        </div>

        <div class="control">
            <label>Color Theme:</label>
            <div class="theme-buttons">
                <button class="theme-btn active" data-theme="rainbow">🌈 Rainbow</button>
                <button class="theme-btn" data-theme="ocean">🌊 Ocean</button>
                <button class="theme-btn" data-theme="fire">🔥 Fire</button>
                <button class="theme-btn" data-theme="neon">💜 Neon</button>
                <button class="theme-btn" data-theme="matrix">💚 Matrix</button>
            </div>
        </div>

        <div class="control">
            <label>
                <input type="checkbox" id="show-lines" checked> Show connecting lines
            </label>
        </div>

        <div class="control">
            <label>
                <input type="checkbox" id="mouse-interact" checked> Mouse interaction
            </label>
        </div>

        <button id="reset-btn" class="reset-btn">🔄 Reset Particles</button>
    </div>

    <!-- Canvas -->
    <canvas id="particle-canvas"></canvas>

    <script src="script.js"></script>
</body>
</html>
```

### Step 2: Add CSS Styling
Create `style.css`:

```css
/* Reset */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    overflow: hidden;
    background: #0a0a0a;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

/* Canvas fills the whole screen */
#particle-canvas {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 1;
}

/* Controls Panel */
.controls-panel {
    position: fixed;
    top: 20px;
    left: 20px;
    z-index: 100;
    background: rgba(20, 20, 40, 0.9);
    backdrop-filter: blur(10px);
    border: 1px solid rgba(255, 255, 255, 0.1);
    border-radius: 15px;
    padding: 20px;
    width: 280px;
    color: white;
    box-shadow: 0 10px 40px rgba(0, 0, 0, 0.5);
}

.controls-panel h2 {
    text-align: center;
    margin-bottom: 15px;
    font-size: 1.3rem;
    background: linear-gradient(45deg, #ff6b6b, #4ecdc4);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
}

/* Individual control */
.control {
    margin-bottom: 15px;
}

.control label {
    display: block;
    font-size: 0.85rem;
    color: #aaa;
    margin-bottom: 6px;
}

/* Range sliders */
input[type="range"] {
    width: 100%;
    height: 6px;
    -webkit-appearance: none;
    appearance: none;
    border-radius: 3px;
    background: rgba(255, 255, 255, 0.2);
    outline: none;
}

input[type="range"]::-webkit-slider-thumb {
    -webkit-appearance: none;
    appearance: none;
    width: 16px;
    height: 16px;
    border-radius: 50%;
    background: #4ecdc4;
    cursor: pointer;
    box-shadow: 0 0 10px rgba(78, 205, 196, 0.5);
}

/* Theme buttons */
.theme-buttons {
    display: flex;
    flex-wrap: wrap;
    gap: 6px;
    margin-top: 6px;
}

.theme-btn {
    padding: 5px 10px;
    border: 1px solid rgba(255, 255, 255, 0.2);
    border-radius: 6px;
    background: rgba(255, 255, 255, 0.05);
    color: white;
    font-size: 0.75rem;
    cursor: pointer;
    transition: all 0.2s ease;
}

.theme-btn:hover {
    background: rgba(255, 255, 255, 0.15);
    border-color: rgba(255, 255, 255, 0.4);
}

.theme-btn.active {
    background: rgba(78, 205, 196, 0.3);
    border-color: #4ecdc4;
}

/* Checkboxes */
input[type="checkbox"] {
    width: 16px;
    height: 16px;
    cursor: pointer;
    vertical-align: middle;
    margin-right: 5px;
}

/* Reset button */
.reset-btn {
    width: 100%;
    padding: 10px;
    border: none;
    border-radius: 8px;
    background: linear-gradient(45deg, #3498db, #2980b9);
    color: white;
    font-size: 0.9rem;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.2s ease;
}

.reset-btn:hover {
    transform: translateY(-2px);
    box-shadow: 0 5px 15px rgba(52, 152, 219, 0.4);
}

/* Responsive - hide panel on very small screens */
@media (max-width: 400px) {
    .controls-panel {
        width: 220px;
        padding: 12px;
        font-size: 0.8rem;
    }
}
```

### Step 3: Add JavaScript (The Animation Engine!)
Create `script.js`:

```javascript
// ===== SETUP CANVAS =====
const canvas = document.getElementById("particle-canvas");
const ctx = canvas.getContext("2d");

// Make canvas fill the screen
function resizeCanvas() {
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;
}
resizeCanvas();
window.addEventListener("resize", resizeCanvas);

// ===== SETTINGS =====
let settings = {
    particleCount: 80,
    speed: 2,
    size: 3,
    connectDistance: 120,
    showLines: true,
    mouseInteract: true,
    theme: "rainbow"
};

// ===== MOUSE TRACKING =====
let mouse = {
    x: canvas.width / 2,
    y: canvas.height / 2
};

canvas.addEventListener("mousemove", function (e) {
    mouse.x = e.clientX;
    mouse.y = e.clientY;
});

canvas.addEventListener("touchmove", function (e) {
    e.preventDefault();
    mouse.x = e.touches[0].clientX;
    mouse.y = e.touches[0].clientY;
});

// ===== COLOR THEMES =====
const themes = {
    rainbow: ["#ff6b6b", "#ffd93d", "#6bcb77", "#4d96ff", "#9b59b6", "#ff9ff3"],
    ocean: ["#0077b6", "#00b4d8", "#48cae4", "#90e0ef", "#ade8f4", "#caf0f8"],
    fire: ["#ffbe0b", "#fb5607", "#ff006e", "#ff4800", "#ff8500", "#ffdd00"],
    neon: ["#9b59b6", "#8e44ad", "#e74c3c", "#f39c12", "#1abc9c", "#3498db"],
    matrix: ["#00ff00", "#00cc00", "#009900", "#006600", "#00ff66", "#33ff33"]
};

// ===== PARTICLE CLASS =====
function createParticle() {
    const themeColors = themes[settings.theme];
    const color = themeColors[Math.floor(Math.random() * themeColors.length)];

    return {
        x: Math.random() * canvas.width,
        y: Math.random() * canvas.height,
        speedX: (Math.random() - 0.5) * settings.speed,
        speedY: (Math.random() - 0.5) * settings.speed,
        size: Math.random() * settings.size + 1,
        color: color,
        opacity: Math.random() * 0.5 + 0.5
    };
}

// ===== CREATE PARTICLES =====
let particles = [];

function initParticles() {
    particles = [];
    for (let i = 0; i < settings.particleCount; i++) {
        particles.push(createParticle());
    }
}

// ===== UPDATE PARTICLE POSITION =====
function updateParticle(particle) {
    // Move the particle
    particle.x += particle.speedX;
    particle.y += particle.speedY;

    // Bounce off walls
    if (particle.x < 0 || particle.x > canvas.width) {
        particle.speedX = -particle.speedX;
    }
    if (particle.y < 0 || particle.y > canvas.height) {
        particle.speedY = -particle.speedY;
    }

    // Mouse interaction - particles move away from mouse
    if (settings.mouseInteract) {
        const dx = particle.x - mouse.x;
        const dy = particle.y - mouse.y;
        const distance = Math.sqrt(dx * dx + dy * dy);

        if (distance < 100) {
            // Push particle away from mouse
            particle.x += dx / distance * 2;
            particle.y += dy / distance * 2;
        }
    }
}

// ===== DRAW A PARTICLE =====
function drawParticle(particle) {
    ctx.beginPath();
    ctx.arc(particle.x, particle.y, particle.size, 0, Math.PI * 2);
    ctx.fillStyle = particle.color;
    ctx.globalAlpha = particle.opacity;
    ctx.fill();
    ctx.globalAlpha = 1;
}

// ===== DRAW CONNECTING LINES =====
function drawLines() {
    for (let i = 0; i < particles.length; i++) {
        for (let j = i + 1; j < particles.length; j++) {
            const dx = particles[i].x - particles[j].x;
            const dy = particles[i].y - particles[j].y;
            const distance = Math.sqrt(dx * dx + dy * dy);

            if (distance < settings.connectDistance) {
                // Fade line based on distance
                const opacity = 1 - (distance / settings.connectDistance);
                ctx.beginPath();
                ctx.moveTo(particles[i].x, particles[i].y);
                ctx.lineTo(particles[j].x, particles[j].y);
                ctx.strokeStyle = particles[i].color;
                ctx.globalAlpha = opacity * 0.3;
                ctx.lineWidth = 1;
                ctx.stroke();
                ctx.globalAlpha = 1;
            }
        }
    }
}

// ===== DRAW LINES TO MOUSE =====
function drawMouseLines() {
    if (!settings.mouseInteract) return;

    for (let i = 0; i < particles.length; i++) {
        const dx = particles[i].x - mouse.x;
        const dy = particles[i].y - mouse.y;
        const distance = Math.sqrt(dx * dx + dy * dy);

        if (distance < settings.connectDistance * 1.5) {
            const opacity = 1 - (distance / (settings.connectDistance * 1.5));
            ctx.beginPath();
            ctx.moveTo(particles[i].x, particles[i].y);
            ctx.lineTo(mouse.x, mouse.y);
            ctx.strokeStyle = particles[i].color;
            ctx.globalAlpha = opacity * 0.5;
            ctx.lineWidth = 1.5;
            ctx.stroke();
            ctx.globalAlpha = 1;
        }
    }
}

// ===== ANIMATION LOOP =====
function animate() {
    // Clear the canvas
    ctx.fillStyle = "rgba(10, 10, 10, 0.1)";
    ctx.fillRect(0, 0, canvas.width, canvas.height);

    // Update and draw each particle
    for (let i = 0; i < particles.length; i++) {
        updateParticle(particles[i]);
        drawParticle(particles[i]);
    }

    // Draw connecting lines
    if (settings.showLines) {
        drawLines();
        drawMouseLines();
    }

    // Loop the animation
    requestAnimationFrame(animate);
}

// ===== CONTROL PANEL EVENT LISTENERS =====

// Particle count
const countSlider = document.getElementById("particle-count");
const countDisplay = document.getElementById("count-display");
countSlider.addEventListener("input", function () {
    settings.particleCount = parseInt(countSlider.value);
    countDisplay.textContent = countSlider.value;
    initParticles();
});

// Speed
const speedSlider = document.getElementById("particle-speed");
const speedDisplay = document.getElementById("speed-display");
speedSlider.addEventListener("input", function () {
    settings.speed = parseInt(speedSlider.value);
    speedDisplay.textContent = speedSlider.value;
    // Update existing particle speeds
    particles.forEach(function (p) {
        p.speedX = (Math.random() - 0.5) * settings.speed;
        p.speedY = (Math.random() - 0.5) * settings.speed;
    });
});

// Size
const sizeSlider = document.getElementById("particle-size");
const sizeDisplay = document.getElementById("size-display");
sizeSlider.addEventListener("input", function () {
    settings.size = parseInt(sizeSlider.value);
    sizeDisplay.textContent = sizeSlider.value;
    particles.forEach(function (p) {
        p.size = Math.random() * settings.size + 1;
    });
});

// Connect distance
const distanceSlider = document.getElementById("connect-distance");
const distanceDisplay = document.getElementById("distance-display");
distanceSlider.addEventListener("input", function () {
    settings.connectDistance = parseInt(distanceSlider.value);
    distanceDisplay.textContent = distanceSlider.value;
});

// Show lines checkbox
const showLinesCheckbox = document.getElementById("show-lines");
showLinesCheckbox.addEventListener("change", function () {
    settings.showLines = showLinesCheckbox.checked;
});

// Mouse interaction checkbox
const mouseCheckbox = document.getElementById("mouse-interact");
mouseCheckbox.addEventListener("change", function () {
    settings.mouseInteract = mouseCheckbox.checked;
});

// Theme buttons
const themeButtons = document.querySelectorAll(".theme-btn");
themeButtons.forEach(function (btn) {
    btn.addEventListener("click", function () {
        themeButtons.forEach(function (b) { b.classList.remove("active"); });
        btn.classList.add("active");
        settings.theme = btn.getAttribute("data-theme");

        // Update particle colors
        const themeColors = themes[settings.theme];
        particles.forEach(function (p) {
            p.color = themeColors[Math.floor(Math.random() * themeColors.length)];
        });
    });
});

// Reset button
document.getElementById("reset-btn").addEventListener("click", function () {
    initParticles();
});

// ===== START =====
initParticles();
animate();
```

---

## 💡 Understanding the Code

### What is Canvas?
Canvas is like a blank piece of paper in your browser. You use JavaScript to draw on it:
```javascript
// Get the canvas and its drawing tools
const canvas = document.getElementById("particle-canvas");
const ctx = canvas.getContext("2d"); // "ctx" = context = drawing tools

// Draw a circle
ctx.beginPath();
ctx.arc(100, 100, 20, 0, Math.PI * 2); // x, y, radius, start angle, end angle
ctx.fill();
```

### How Animation Works:
```javascript
function animate() {
    // 1. Clear the screen (or fade it slightly)
    // 2. Update positions
    // 3. Draw everything
    // 4. Repeat!
    requestAnimationFrame(animate);
}
```
`requestAnimationFrame` tells the browser "call this function again next frame" — it runs ~60 times per second!

### Key Math Concepts:
| Formula | What It Does |
|---------|--------------|
| `Math.random()` | Random number between 0 and 1 |
| `Math.sqrt(dx*dx + dy*dy)` | Distance between two points |
| `Math.PI * 2` | Full circle in radians |

---

## 🎨 Creative Challenges

### Challenge 1: Click to Add Particles
Add particles wherever you click:
```javascript
canvas.addEventListener("click", function(e) {
    for (let i = 0; i < 5; i++) {
        let p = createParticle();
        p.x = e.clientX;
        p.y = e.clientY;
        particles.push(p);
    }
});
```

### Challenge 2: Gravity Mode
Make particles fall down and bounce:
```javascript
// Inside updateParticle:
particle.speedY += 0.05; // gravity pulls down
if (particle.y > canvas.height) {
    particle.y = canvas.height;
    particle.speedY = -particle.speedY * 0.8; // bounce with energy loss
}
```

### Challenge 3: Trail Effect
Instead of clearing the canvas completely, use a semi-transparent overlay:
```javascript
// Replace ctx.clearRect with:
ctx.fillStyle = "rgba(10, 10, 10, 0.05)"; // Very faint, creates trails
ctx.fillRect(0, 0, canvas.width, canvas.height);
```

### Challenge 4: Fireworks Mode
On click, spawn particles that explode outward:
```javascript
function firework(x, y) {
    for (let i = 0; i < 30; i++) {
        const angle = (Math.PI * 2 / 30) * i;
        const speed = Math.random() * 5 + 2;
        let p = createParticle();
        p.x = x;
        p.y = y;
        p.speedX = Math.cos(angle) * speed;
        p.speedY = Math.sin(angle) * speed;
        particles.push(p);
    }
}
```

### Challenge 5: Custom Shapes
Instead of circles, draw squares or stars:
```javascript
// Square particle
ctx.fillRect(particle.x - particle.size, particle.y - particle.size, particle.size * 2, particle.size * 2);
```

---

## 🔍 What Did You Learn?
- HTML Canvas for 2D drawing
- Animation loops with requestAnimationFrame
- Particle systems (used in real game engines!)
- Distance calculations
- Real-time user interaction
- Performance-friendly rendering
- How modern website backgrounds are made

## 🎉 Congratulations!
You've completed Day 2! You now know how to:
- Create interactive drawing tools (Pixel Art Editor)
- Build useful design tools (Gradient Designer)
- Make stunning animations (Particle Canvas)

These same concepts are used in:
- Video games (particle effects, animations)
- Graphic design software (Photoshop, Figma)
- Modern websites (background animations)
- Mobile apps (interactive UI)

Keep experimenting! Change values, break things, fix them, and create something uniquely yours. 🚀
