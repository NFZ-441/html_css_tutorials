# Tutorial 2: Gradient Designer Tool 🌈

## 🎯 Objective
Build a live gradient designer where you pick colors, choose direction, and see the gradient update instantly. You'll even be able to copy the CSS code to use in your own projects!

## 📚 What You'll Learn
- Real-time updates (changing things as you interact)
- CSS gradients and how they work
- Input events and range sliders
- Copying text to clipboard with JavaScript
- Template literals (fancy strings)

## 🖥️ How to Run This
**Option A:** Create files in vscode.dev and use Live Preview
**Option B:** Paste all the code into [CodePen.io](https://codepen.io)

---

## 📖 Concepts You'll Use (Read This First!)

### 1. Input Events — Reacting Instantly
Unlike a "click", an "input" event fires *every time* a slider or color picker changes — even while you're dragging. This is how we get live updates.
```javascript
slider.addEventListener("input", function() {
    // This fires continuously as you drag the slider
    // Perfect for real-time previews!
});
```

### 2. Template Strings — Building Text from Variables
You often need to combine text with variables. The `+` operator joins them together:
```javascript
let angle = 90;
let result = "rotate(" + angle + "deg)";
// result is: "rotate(90deg)"

let r = 255;
let g = 100;
let b = 50;
let color = "rgb(" + r + "," + g + "," + b + ")";
// color is: "rgb(255,100,50)"
```

### 3. getAttribute() — Reading HTML Data
You can store custom data in HTML attributes and read them with JavaScript:
```html
<button data-direction="to right">→</button>
```
```javascript
let direction = button.getAttribute("data-direction");
// direction is: "to right"
```

### 4. Clipboard API — Copying Text
Modern browsers let you copy text to the clipboard with one line:
```javascript
navigator.clipboard.writeText("Hello!");
// Now "Hello!" is copied — user can paste it anywhere
```

### 5. setTimeout() — Delayed Actions
Run code after a delay (in milliseconds):
```javascript
setTimeout(function() {
    alert("This appears after 2 seconds!");
}, 2000);  // 2000ms = 2 seconds
```

### How These Concepts Connect in This Project:
| Concept | How We Use It |
|---------|---------------|
| Input Events | Update gradient preview live as you adjust sliders |
| Template Strings | Build CSS gradient code like `linear-gradient(90deg, #ff0000, #00ff00)` |
| getAttribute() | Read which direction button was clicked |
| Clipboard API | Let users copy the CSS code with one click |
| setTimeout() | Reset the "Copied!" button text after 2 seconds |

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
    <title>Gradient Designer</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="app-container">
        <header class="app-header">
            <h1>🌈 Gradient Designer</h1>
            <p class="tagline">Create beautiful gradients and copy the CSS!</p>
        </header>

        <!-- Gradient Preview -->
        <div id="gradient-preview" class="gradient-preview"></div>

        <!-- Controls -->
        <div class="controls">
            <!-- Color Pickers -->
            <div class="control-row">
                <div class="control-group">
                    <label for="color1">Color 1:</label>
                    <input type="color" id="color1" value="#ff6b6b">
                    <span id="color1-hex" class="hex-value">#ff6b6b</span>
                </div>

                <div class="control-group">
                    <label for="color2">Color 2:</label>
                    <input type="color" id="color2" value="#4ecdc4">
                    <span id="color2-hex" class="hex-value">#4ecdc4</span>
                </div>
            </div>

            <!-- Optional Third Color -->
            <div class="control-row">
                <div class="control-group">
                    <label>
                        <input type="checkbox" id="use-color3"> Add 3rd Color:
                    </label>
                    <input type="color" id="color3" value="#ffd93d" disabled>
                    <span id="color3-hex" class="hex-value">#ffd93d</span>
                </div>
            </div>

            <!-- Direction -->
            <div class="control-row">
                <div class="control-group full-width">
                    <label for="direction">Direction:</label>
                    <div class="direction-buttons">
                        <button class="dir-btn active" data-direction="to right">→</button>
                        <button class="dir-btn" data-direction="to left">←</button>
                        <button class="dir-btn" data-direction="to bottom">↓</button>
                        <button class="dir-btn" data-direction="to top">↑</button>
                        <button class="dir-btn" data-direction="to bottom right">↘</button>
                        <button class="dir-btn" data-direction="to top left">↖</button>
                        <button class="dir-btn" data-direction="to bottom left">↙</button>
                        <button class="dir-btn" data-direction="to top right">↗</button>
                    </div>
                </div>
            </div>

            <!-- Angle Slider -->
            <div class="control-row">
                <div class="control-group full-width">
                    <label for="angle">Custom Angle: <span id="angle-value">90</span>°</label>
                    <input type="range" id="angle" min="0" max="360" value="90" class="slider">
                </div>
            </div>

            <!-- Gradient Type -->
            <div class="control-row">
                <div class="control-group full-width">
                    <label>Type:</label>
                    <div class="type-buttons">
                        <button class="type-btn active" data-type="linear">Linear</button>
                        <button class="type-btn" data-type="radial">Radial</button>
                        <button class="type-btn" data-type="conic">Conic</button>
                    </div>
                </div>
            </div>
        </div>

        <!-- CSS Output -->
        <div class="css-output">
            <div class="output-header">
                <h3>📋 CSS Code:</h3>
                <button id="copy-btn" class="copy-btn">Copy CSS</button>
            </div>
            <code id="css-code" class="code-display">background: linear-gradient(to right, #ff6b6b, #4ecdc4);</code>
        </div>

        <!-- Preset Gradients -->
        <div class="presets">
            <h3>✨ Preset Gradients (click to apply):</h3>
            <div class="preset-grid">
                <div class="preset" data-colors="#ff9a9e,#fecfef" style="background: linear-gradient(to right, #ff9a9e, #fecfef);"></div>
                <div class="preset" data-colors="#a18cd1,#fbc2eb" style="background: linear-gradient(to right, #a18cd1, #fbc2eb);"></div>
                <div class="preset" data-colors="#fad0c4,#ffd1ff" style="background: linear-gradient(to right, #fad0c4, #ffd1ff);"></div>
                <div class="preset" data-colors="#84fab0,#8fd3f4" style="background: linear-gradient(to right, #84fab0, #8fd3f4);"></div>
                <div class="preset" data-colors="#fa709a,#fee140" style="background: linear-gradient(to right, #fa709a, #fee140);"></div>
                <div class="preset" data-colors="#667eea,#764ba2" style="background: linear-gradient(to right, #667eea, #764ba2);"></div>
                <div class="preset" data-colors="#f093fb,#f5576c" style="background: linear-gradient(to right, #f093fb, #f5576c);"></div>
                <div class="preset" data-colors="#4facfe,#00f2fe" style="background: linear-gradient(to right, #4facfe, #00f2fe);"></div>
                <div class="preset" data-colors="#43e97b,#38f9d7" style="background: linear-gradient(to right, #43e97b, #38f9d7);"></div>
                <div class="preset" data-colors="#0c3483,#a2b6df" style="background: linear-gradient(to right, #0c3483, #a2b6df);"></div>
            </div>
        </div>
    </div>

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
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background: #1a1a2e;
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: flex-start;
    padding: 20px;
}

/* App Container */
.app-container {
    background: white;
    border-radius: 20px;
    box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
    padding: 30px;
    max-width: 650px;
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
}

.tagline {
    color: #666;
    font-size: 0.9rem;
}

/* Gradient Preview */
.gradient-preview {
    width: 100%;
    height: 200px;
    border-radius: 15px;
    margin-bottom: 20px;
    background: linear-gradient(to right, #ff6b6b, #4ecdc4);
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
    transition: all 0.3s ease;
}

/* Controls */
.controls {
    background: #f8f9fa;
    border-radius: 12px;
    padding: 20px;
    margin-bottom: 20px;
}

.control-row {
    display: flex;
    flex-wrap: wrap;
    gap: 15px;
    margin-bottom: 15px;
    align-items: center;
}

.control-row:last-child {
    margin-bottom: 0;
}

.control-group {
    display: flex;
    align-items: center;
    gap: 10px;
}

.control-group.full-width {
    width: 100%;
    flex-direction: column;
    align-items: flex-start;
}

.control-group label {
    font-weight: 600;
    color: #333;
    font-size: 0.9rem;
}

.hex-value {
    font-family: monospace;
    font-size: 0.85rem;
    color: #666;
    background: #eee;
    padding: 3px 8px;
    border-radius: 4px;
}

/* Direction Buttons */
.direction-buttons,
.type-buttons {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
    margin-top: 8px;
}

.dir-btn,
.type-btn {
    width: 40px;
    height: 40px;
    border: 2px solid #ddd;
    border-radius: 8px;
    background: white;
    cursor: pointer;
    font-size: 1.2rem;
    transition: all 0.2s ease;
    display: flex;
    align-items: center;
    justify-content: center;
}

.type-btn {
    width: auto;
    padding: 8px 16px;
    font-size: 0.9rem;
}

.dir-btn:hover,
.type-btn:hover {
    border-color: #3498db;
    background: #ebf5fb;
}

.dir-btn.active,
.type-btn.active {
    border-color: #3498db;
    background: #3498db;
    color: white;
}

/* Slider */
.slider {
    width: 100%;
    margin-top: 8px;
    height: 8px;
    -webkit-appearance: none;
    appearance: none;
    border-radius: 4px;
    background: linear-gradient(to right, #3498db, #e74c3c);
    outline: none;
}

.slider::-webkit-slider-thumb {
    -webkit-appearance: none;
    appearance: none;
    width: 22px;
    height: 22px;
    border-radius: 50%;
    background: white;
    border: 3px solid #3498db;
    cursor: pointer;
    box-shadow: 0 2px 6px rgba(0, 0, 0, 0.2);
}

/* CSS Output */
.css-output {
    background: #2c3e50;
    border-radius: 12px;
    padding: 15px 20px;
    margin-bottom: 20px;
}

.output-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 10px;
}

.output-header h3 {
    color: white;
    font-size: 0.9rem;
}

.copy-btn {
    padding: 6px 14px;
    background: #27ae60;
    color: white;
    border: none;
    border-radius: 6px;
    cursor: pointer;
    font-weight: 600;
    font-size: 0.85rem;
    transition: all 0.2s ease;
}

.copy-btn:hover {
    background: #2ecc71;
    transform: translateY(-1px);
}

.copy-btn.copied {
    background: #f39c12;
}

.code-display {
    display: block;
    color: #2ecc71;
    font-family: 'Courier New', monospace;
    font-size: 0.85rem;
    word-break: break-all;
    line-height: 1.5;
}

/* Presets */
.presets {
    margin-top: 20px;
}

.presets h3 {
    color: #333;
    font-size: 1rem;
    margin-bottom: 12px;
}

.preset-grid {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    gap: 10px;
}

.preset {
    height: 50px;
    border-radius: 10px;
    cursor: pointer;
    transition: all 0.2s ease;
    border: 3px solid transparent;
}

.preset:hover {
    transform: scale(1.1);
    border-color: #2c3e50;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
}

/* Color input styling */
input[type="color"] {
    width: 40px;
    height: 35px;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    padding: 2px;
}

input[type="color"]:disabled {
    opacity: 0.4;
    cursor: not-allowed;
}

/* Checkbox */
input[type="checkbox"] {
    width: 18px;
    height: 18px;
    cursor: pointer;
}

/* Responsive */
@media (max-width: 500px) {
    .app-container {
        padding: 15px;
    }

    .preset-grid {
        grid-template-columns: repeat(3, 1fr);
    }

    .gradient-preview {
        height: 150px;
    }

    .app-header h1 {
        font-size: 1.5rem;
    }
}
```

### Step 3: Add JavaScript
Create `script.js`:

```javascript
// ===== GET ELEMENTS =====
const preview = document.getElementById("gradient-preview");
const color1Input = document.getElementById("color1");
const color2Input = document.getElementById("color2");
const color3Input = document.getElementById("color3");
const useColor3Checkbox = document.getElementById("use-color3");
const color1Hex = document.getElementById("color1-hex");
const color2Hex = document.getElementById("color2-hex");
const color3Hex = document.getElementById("color3-hex");
const angleSlider = document.getElementById("angle");
const angleValue = document.getElementById("angle-value");
const cssCode = document.getElementById("css-code");
const copyBtn = document.getElementById("copy-btn");
const dirButtons = document.querySelectorAll(".dir-btn");
const typeButtons = document.querySelectorAll(".type-btn");
const presets = document.querySelectorAll(".preset");

// ===== STATE =====
let currentDirection = "to right";
let currentType = "linear";
let useAngle = false;

// ===== UPDATE GRADIENT =====
function updateGradient() {
    const c1 = color1Input.value;
    const c2 = color2Input.value;
    const c3 = color3Input.value;
    const angle = angleSlider.value;
    const useThirdColor = useColor3Checkbox.checked;

    // Build color string
    let colors = c1 + ", " + c2;
    if (useThirdColor) {
        colors = c1 + ", " + c2 + ", " + c3;
    }

    // Build gradient string
    let gradient = "";
    let cssText = "";

    if (currentType === "linear") {
        if (useAngle) {
            gradient = "linear-gradient(" + angle + "deg, " + colors + ")";
        } else {
            gradient = "linear-gradient(" + currentDirection + ", " + colors + ")";
        }
    } else if (currentType === "radial") {
        gradient = "radial-gradient(circle, " + colors + ")";
    } else if (currentType === "conic") {
        gradient = "conic-gradient(from " + angle + "deg, " + colors + ")";
    }

    // Apply to preview
    preview.style.background = gradient;

    // Update CSS code display
    cssText = "background: " + gradient + ";";
    cssCode.textContent = cssText;

    // Update hex displays
    color1Hex.textContent = c1;
    color2Hex.textContent = c2;
    color3Hex.textContent = c3;
}

// ===== EVENT LISTENERS =====

// Color picker changes
color1Input.addEventListener("input", updateGradient);
color2Input.addEventListener("input", updateGradient);
color3Input.addEventListener("input", updateGradient);

// Third color checkbox
useColor3Checkbox.addEventListener("change", function () {
    color3Input.disabled = !useColor3Checkbox.checked;
    updateGradient();
});

// Angle slider
angleSlider.addEventListener("input", function () {
    angleValue.textContent = angleSlider.value;
    useAngle = true;
    // Remove active from direction buttons
    dirButtons.forEach(function (btn) {
        btn.classList.remove("active");
    });
    updateGradient();
});

// Direction buttons
dirButtons.forEach(function (btn) {
    btn.addEventListener("click", function () {
        dirButtons.forEach(function (b) {
            b.classList.remove("active");
        });
        btn.classList.add("active");
        currentDirection = btn.getAttribute("data-direction");
        useAngle = false;
        updateGradient();
    });
});

// Type buttons
typeButtons.forEach(function (btn) {
    btn.addEventListener("click", function () {
        typeButtons.forEach(function (b) {
            b.classList.remove("active");
        });
        btn.classList.add("active");
        currentType = btn.getAttribute("data-type");
        updateGradient();
    });
});

// Copy button
copyBtn.addEventListener("click", function () {
    const text = cssCode.textContent;

    // Copy to clipboard
    navigator.clipboard.writeText(text).then(function () {
        copyBtn.textContent = "✓ Copied!";
        copyBtn.classList.add("copied");

        // Reset button after 2 seconds
        setTimeout(function () {
            copyBtn.textContent = "Copy CSS";
            copyBtn.classList.remove("copied");
        }, 2000);
    });
});

// Preset gradients
presets.forEach(function (preset) {
    preset.addEventListener("click", function () {
        const colors = preset.getAttribute("data-colors").split(",");
        color1Input.value = colors[0];
        color2Input.value = colors[1];
        if (colors[2]) {
            useColor3Checkbox.checked = true;
            color3Input.disabled = false;
            color3Input.value = colors[2];
        }
        updateGradient();
    });
});

// ===== INITIALIZE =====
updateGradient();
```

---

## 💡 Understanding the Code

### Key JavaScript Concepts:

| Concept | Example | What It Does |
|---------|---------|--------------|
| `addEventListener("input")` | Color picker | Fires every time the value changes |
| `getAttribute()` | `btn.getAttribute("data-direction")` | Reads custom HTML attributes |
| Template strings | `angle + "deg"` | Builds text from variables |
| `navigator.clipboard` | Copy button | Copies text to user's clipboard |
| `setTimeout()` | Reset button text | Runs code after a delay |

### How Gradients Work in CSS:
```css
/* Linear: flows in one direction */
background: linear-gradient(to right, red, blue);

/* Radial: flows from center outward */
background: radial-gradient(circle, red, blue);

/* Conic: flows in a circle like a color wheel */
background: conic-gradient(red, blue, red);
```

---

## 🎨 Creative Challenges

### Challenge 1: Add More Colors (4 or 5 stops)
Let users add up to 5 color stops for complex gradients.

### Challenge 2: Animation Toggle
Make the gradient slowly animate and shift colors:
```css
.gradient-preview.animated {
    background-size: 200% 200%;
    animation: gradientShift 3s ease infinite;
}

@keyframes gradientShift {
    0% { background-position: 0% 50%; }
    50% { background-position: 100% 50%; }
    100% { background-position: 0% 50%; }
}
```

### Challenge 3: Random Gradient Button
```javascript
function randomGradient() {
    const randomColor = function() {
        return "#" + Math.floor(Math.random() * 16777215).toString(16).padStart(6, "0");
    };
    color1Input.value = randomColor();
    color2Input.value = randomColor();
    updateGradient();
}
```

### Challenge 4: Save Favorites
Store gradients in an array and display them as a gallery:
```javascript
let favorites = [];

function saveFavorite() {
    favorites.push(cssCode.textContent);
    // Display saved gradients below
}
```

---

## 🔍 What Did You Learn?
- Real-time UI updates
- Working with color values
- CSS gradient syntax (linear, radial, conic)
- Clipboard API
- Managing multiple event listeners
- Building a useful design tool

## 🚀 Next Steps
Try Tutorial 3: Interactive Particle Canvas to learn about animation and the HTML Canvas element!
