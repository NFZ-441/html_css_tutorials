# Tutorial 1: Pixel Art Editor 🎨

## 🎯 Objective
Build a pixel art drawing tool where you click on grid squares to paint them. Choose colors, change grid size, clear your canvas, and create retro-style artwork!

## 📚 What You'll Learn
- JavaScript event listeners (responding to clicks)
- DOM manipulation (changing elements with code)
- CSS Grid for layout
- Variables and functions
- Loops (creating elements with code)

## 🖥️ How to Run This
**Option A:** Create files in vscode.dev and use Live Preview
**Option B:** Paste all the code into [CodePen.io](https://codepen.io) (HTML → HTML panel, CSS → CSS panel, JS → JS panel)

---

## 📖 Concepts You'll Use (Read This First!)

Before we build, let's understand the key ideas. Don't worry about memorizing — you'll see them in action soon!

### 1. Variables — Storing Information
A variable is like a labelled box that holds a value. You can change what's inside later.
```javascript
let color = "red";     // This box holds the word "red"
let size = 16;         // This box holds the number 16
let isEraser = false;  // This box holds true or false

color = "blue";        // Now the box holds "blue" instead
```

### 2. Functions — Reusable Actions
A function is a set of instructions with a name. You "call" it whenever you want those instructions to run.
```javascript
function sayHello() {
    alert("Hello there!");
}

sayHello();  // This runs the instructions inside
sayHello();  // You can call it as many times as you want
```

### 3. Event Listeners — Reacting to User Actions
An event listener waits for something to happen (a click, a mouse move) and then runs code.
```javascript
button.addEventListener("click", function() {
    // This code runs ONLY when the button is clicked
    alert("You clicked me!");
});
```

### 4. The DOM — Talking to the Page
The DOM (Document Object Model) is how JavaScript sees your HTML. You can grab elements and change them:
```javascript
// Grab an element by its ID
let heading = document.getElementById("my-heading");

// Change its content
heading.textContent = "New Title!";

// Change its style
heading.style.backgroundColor = "red";
```

### 5. Loops — Repeating Things
A loop runs the same code over and over. Perfect for creating many similar elements:
```javascript
for (let i = 0; i < 10; i++) {
    // This code runs 10 times
    // i goes: 0, 1, 2, 3, 4, 5, 6, 7, 8, 9
    console.log("Round number: " + i);
}
```

### How These Concepts Connect in This Project:
| Concept | How We Use It |
|---------|---------------|
| Variables | Store the current color and grid size |
| Functions | `createGrid()` builds the grid, `paintPixel()` colors a square |
| Event Listeners | Detect mouse clicks and drags on pixels |
| DOM | Create pixel `<div>` elements and change their colors |
| Loops | Generate 16×16 = 256 pixel squares automatically |

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
    <title>Pixel Art Editor</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="app-container">
        <header class="app-header">
            <h1>🎨 Pixel Art Editor</h1>
            <p class="tagline">Click squares to create pixel art!</p>
        </header>

        <!-- Toolbar -->
        <div class="toolbar">
            <div class="tool-group">
                <label for="color-picker">🎨 Color:</label>
                <input type="color" id="color-picker" value="#ff6b6b">
            </div>

            <div class="tool-group">
                <label for="grid-size">📐 Grid Size:</label>
                <select id="grid-size">
                    <option value="8">8 x 8</option>
                    <option value="12">12 x 12</option>
                    <option value="16" selected>16 x 16</option>
                    <option value="24">24 x 24</option>
                    <option value="32">32 x 32</option>
                </select>
            </div>

            <div class="tool-group">
                <button id="eraser-btn" class="tool-btn">🧹 Eraser</button>
                <button id="clear-btn" class="tool-btn danger">🗑️ Clear All</button>
            </div>
        </div>

        <!-- Color Palette -->
        <div class="color-palette">
            <div class="palette-color" style="background: #ff6b6b;" data-color="#ff6b6b"></div>
            <div class="palette-color" style="background: #ffd93d;" data-color="#ffd93d"></div>
            <div class="palette-color" style="background: #6bcb77;" data-color="#6bcb77"></div>
            <div class="palette-color" style="background: #4d96ff;" data-color="#4d96ff"></div>
            <div class="palette-color" style="background: #9b59b6;" data-color="#9b59b6"></div>
            <div class="palette-color" style="background: #ff9ff3;" data-color="#ff9ff3"></div>
            <div class="palette-color" style="background: #feca57;" data-color="#feca57"></div>
            <div class="palette-color" style="background: #48dbfb;" data-color="#48dbfb"></div>
            <div class="palette-color" style="background: #ff9f43;" data-color="#ff9f43"></div>
            <div class="palette-color" style="background: #2c3e50;" data-color="#2c3e50"></div>
            <div class="palette-color" style="background: #ffffff;" data-color="#ffffff"></div>
            <div class="palette-color" style="background: #000000;" data-color="#000000"></div>
        </div>

        <!-- Pixel Grid -->
        <div id="pixel-grid" class="pixel-grid"></div>

        <!-- Status Bar -->
        <div class="status-bar">
            <span id="current-tool">🎨 Drawing</span>
            <span id="current-color-display">Color: <span id="color-preview" class="color-preview"></span></span>
        </div>
    </div>

    <script src="script.js"></script>
</body>
</html>
```

### Step 2: Add CSS Styling
Create `style.css`:

```css
/* Reset and base styles */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background: linear-gradient(135deg, #1a1a2e 0%, #16213e 50%, #0f3460 100%);
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 20px;
}

/* App Container */
.app-container {
    background: rgba(255, 255, 255, 0.95);
    border-radius: 20px;
    box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
    padding: 30px;
    max-width: 700px;
    width: 100%;
}

/* Header */
.app-header {
    text-align: center;
    margin-bottom: 20px;
}

.app-header h1 {
    font-size: 2rem;
    color: #2c3e50;
    margin-bottom: 5px;
}

.tagline {
    color: #666;
    font-size: 0.9rem;
}

/* Toolbar */
.toolbar {
    display: flex;
    flex-wrap: wrap;
    gap: 15px;
    align-items: center;
    justify-content: center;
    padding: 15px;
    background: #f8f9fa;
    border-radius: 12px;
    margin-bottom: 15px;
}

.tool-group {
    display: flex;
    align-items: center;
    gap: 8px;
}

.tool-group label {
    font-size: 0.9rem;
    font-weight: 600;
    color: #333;
}

#color-picker {
    width: 40px;
    height: 35px;
    border: none;
    border-radius: 8px;
    cursor: pointer;
}

#grid-size {
    padding: 8px 12px;
    border: 2px solid #ddd;
    border-radius: 8px;
    font-size: 0.9rem;
    cursor: pointer;
}

.tool-btn {
    padding: 8px 16px;
    border: none;
    border-radius: 8px;
    font-size: 0.9rem;
    cursor: pointer;
    font-weight: 600;
    transition: all 0.2s ease;
    background: #3498db;
    color: white;
}

.tool-btn:hover {
    transform: translateY(-2px);
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}

.tool-btn.danger {
    background: #e74c3c;
}

.tool-btn.danger:hover {
    background: #c0392b;
}

.tool-btn.active {
    background: #2c3e50;
    box-shadow: 0 0 0 3px #3498db;
}

/* Color Palette */
.color-palette {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
    justify-content: center;
    margin-bottom: 15px;
    padding: 10px;
}

.palette-color {
    width: 30px;
    height: 30px;
    border-radius: 50%;
    cursor: pointer;
    border: 3px solid #ddd;
    transition: all 0.2s ease;
}

.palette-color:hover {
    transform: scale(1.3);
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
}

.palette-color.selected {
    border-color: #2c3e50;
    transform: scale(1.2);
    box-shadow: 0 0 0 3px #3498db;
}

/* Pixel Grid */
.pixel-grid {
    display: grid;
    gap: 1px;
    background: #ccc;
    border: 3px solid #2c3e50;
    border-radius: 8px;
    overflow: hidden;
    margin: 0 auto;
    max-width: 100%;
    aspect-ratio: 1;
}

.pixel {
    background: white;
    cursor: pointer;
    transition: background-color 0.1s ease;
}

.pixel:hover {
    opacity: 0.7;
    outline: 2px solid #3498db;
    outline-offset: -2px;
}

/* Status Bar */
.status-bar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-top: 15px;
    padding: 10px 15px;
    background: #f8f9fa;
    border-radius: 8px;
    font-size: 0.85rem;
    color: #555;
}

.color-preview {
    display: inline-block;
    width: 16px;
    height: 16px;
    border-radius: 4px;
    border: 1px solid #ccc;
    vertical-align: middle;
}

/* Responsive */
@media (max-width: 600px) {
    .app-container {
        padding: 15px;
    }

    .toolbar {
        flex-direction: column;
    }

    .app-header h1 {
        font-size: 1.5rem;
    }
}
```

### Step 3: Add JavaScript (The Magic!)
Create `script.js`:

```javascript
// ===== VARIABLES =====
let currentColor = "#ff6b6b";
let isEraser = false;
let isMouseDown = false;
let gridSize = 16;

// ===== GET ELEMENTS =====
const grid = document.getElementById("pixel-grid");
const colorPicker = document.getElementById("color-picker");
const gridSizeSelect = document.getElementById("grid-size");
const eraserBtn = document.getElementById("eraser-btn");
const clearBtn = document.getElementById("clear-btn");
const currentToolDisplay = document.getElementById("current-tool");
const colorPreview = document.getElementById("color-preview");
const paletteColors = document.querySelectorAll(".palette-color");

// ===== CREATE THE GRID =====
function createGrid(size) {
    // Clear existing grid
    grid.innerHTML = "";

    // Set grid columns
    grid.style.gridTemplateColumns = "repeat(" + size + ", 1fr)";
    grid.style.gridTemplateRows = "repeat(" + size + ", 1fr)";

    // Create each pixel
    for (let i = 0; i < size * size; i++) {
        const pixel = document.createElement("div");
        pixel.classList.add("pixel");

        // When you click a pixel, color it
        pixel.addEventListener("mousedown", function () {
            paintPixel(pixel);
        });

        // When you drag over a pixel (holding mouse), color it
        pixel.addEventListener("mouseover", function () {
            if (isMouseDown) {
                paintPixel(pixel);
            }
        });

        // Touch support for Chromebooks
        pixel.addEventListener("touchstart", function (e) {
            e.preventDefault();
            paintPixel(pixel);
        });

        pixel.addEventListener("touchmove", function (e) {
            e.preventDefault();
            const touch = e.touches[0];
            const element = document.elementFromPoint(touch.clientX, touch.clientY);
            if (element && element.classList.contains("pixel")) {
                paintPixel(element);
            }
        });

        grid.appendChild(pixel);
    }
}

// ===== PAINT A PIXEL =====
function paintPixel(pixel) {
    if (isEraser) {
        pixel.style.backgroundColor = "white";
    } else {
        pixel.style.backgroundColor = currentColor;
    }
}

// ===== EVENT LISTENERS =====

// Track mouse button state (for drag painting)
document.addEventListener("mousedown", function () {
    isMouseDown = true;
});

document.addEventListener("mouseup", function () {
    isMouseDown = false;
});

// Color picker changes
colorPicker.addEventListener("input", function () {
    currentColor = colorPicker.value;
    isEraser = false;
    eraserBtn.classList.remove("active");
    currentToolDisplay.textContent = "🎨 Drawing";
    updateColorPreview();
});

// Grid size changes
gridSizeSelect.addEventListener("change", function () {
    gridSize = parseInt(gridSizeSelect.value);
    createGrid(gridSize);
});

// Eraser button
eraserBtn.addEventListener("click", function () {
    isEraser = !isEraser;
    if (isEraser) {
        eraserBtn.classList.add("active");
        currentToolDisplay.textContent = "🧹 Erasing";
    } else {
        eraserBtn.classList.remove("active");
        currentToolDisplay.textContent = "🎨 Drawing";
    }
});

// Clear button
clearBtn.addEventListener("click", function () {
    const pixels = document.querySelectorAll(".pixel");
    pixels.forEach(function (pixel) {
        pixel.style.backgroundColor = "white";
    });
});

// Palette colors
paletteColors.forEach(function (paletteColor) {
    paletteColor.addEventListener("click", function () {
        // Remove selected class from all
        paletteColors.forEach(function (pc) {
            pc.classList.remove("selected");
        });
        // Add selected class to clicked one
        paletteColor.classList.add("selected");
        // Set current color
        currentColor = paletteColor.getAttribute("data-color");
        colorPicker.value = currentColor;
        isEraser = false;
        eraserBtn.classList.remove("active");
        currentToolDisplay.textContent = "🎨 Drawing";
        updateColorPreview();
    });
});

// ===== HELPER FUNCTIONS =====
function updateColorPreview() {
    colorPreview.style.backgroundColor = currentColor;
}

// ===== START THE APP =====
createGrid(gridSize);
updateColorPreview();
```

---

## 💡 Understanding the Code

### HTML Concepts Used:
- `<input type="color">` — a color picker built into the browser!
- `<select>` and `<option>` — dropdown menus
- `data-color` attribute — custom data we attach to elements

### CSS Concepts Used:
- `display: grid` — creates the pixel grid
- `aspect-ratio: 1` — keeps the grid square
- `transition` — smooth color changes
- CSS variables for theming

### JavaScript Concepts Used:
| Concept | What It Does |
|---------|--------------|
| `let` | Creates a variable that can change |
| `document.getElementById()` | Grabs an element from the page |
| `addEventListener()` | Listens for clicks, mouse moves, etc. |
| `createElement()` | Creates a new HTML element with code |
| `appendChild()` | Adds an element to the page |
| `classList.add/remove()` | Adds or removes CSS classes |
| `style.backgroundColor` | Changes an element's background color |
| `for` loop | Repeats code many times |

---

## 🎨 Creative Challenges

### Challenge 1: Add a Fill Tool
Fill an entire area with one color:
```javascript
// Hint: Change all white pixels to current color
function fillAll() {
    const pixels = document.querySelectorAll(".pixel");
    pixels.forEach(function(pixel) {
        if (pixel.style.backgroundColor === "white" || pixel.style.backgroundColor === "") {
            pixel.style.backgroundColor = currentColor;
        }
    });
}
```

### Challenge 2: Random Color Mode
Add a button that uses a random color for each pixel:
```javascript
function getRandomColor() {
    const letters = "0123456789ABCDEF";
    let color = "#";
    for (let i = 0; i < 6; i++) {
        color += letters[Math.floor(Math.random() * 16)];
    }
    return color;
}
```

### Challenge 3: Add Grid Lines Toggle
```javascript
function toggleGridLines() {
    if (grid.style.gap === "1px") {
        grid.style.gap = "0px";
    } else {
        grid.style.gap = "1px";
    }
}
```

### Challenge 4: Pre-made Templates
Add buttons that load pixel art templates:
```javascript
// A simple heart pattern for an 8x8 grid
const heartPattern = [
    0,1,1,0,0,1,1,0,
    1,1,1,1,1,1,1,1,
    1,1,1,1,1,1,1,1,
    1,1,1,1,1,1,1,1,
    0,1,1,1,1,1,1,0,
    0,0,1,1,1,1,0,0,
    0,0,0,1,1,0,0,0,
    0,0,0,0,0,0,0,0
];
```

### Challenge 5: Dark Mode
Add a toggle that switches the app to dark mode:
```css
.dark-mode .app-container {
    background: #1a1a2e;
    color: white;
}
```

---

## 🔍 What Did You Learn?
- How JavaScript makes pages interactive
- Event listeners (clicks, mouse movement)
- Creating HTML elements with JavaScript
- Loops for repetitive tasks
- How variables store changing information
- DOM manipulation basics

## 🚀 Next Steps
Try Tutorial 2: Gradient Designer Tool to learn about real-time updates and creating useful design tools!
