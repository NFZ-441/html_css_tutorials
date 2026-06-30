# Tutorial 4: Color Match Game 🎯

## 🎯 Objective
Build a game where you're shown a target color and have to mix RGB sliders to match it as closely as possible. Perfect for graphic designers — you'll train your eye for color!

## 📚 What You'll Learn
- How RGB colors work (Red, Green, Blue values 0-255)
- Score systems and game logic
- Comparing values with JavaScript
- Timer functions
- Simple game loop

## 🖥️ How to Run This
**Option A:** Create files in vscode.dev and use Live Preview
**Option B:** Paste all the code into [CodePen.io](https://codepen.io)

---

## 📖 Concepts You'll Use (Read This First!)

### 1. RGB Colors — How Screens Make Color
Every color on screen is made by mixing **Red**, **Green**, and **Blue** light. Each channel goes from 0 (none) to 255 (full):
```
Red   = rgb(255, 0, 0)      → Pure red light
Green = rgb(0, 255, 0)      → Pure green light
Blue  = rgb(0, 0, 255)      → Pure blue light
White = rgb(255, 255, 255)  → All lights on full
Black = rgb(0, 0, 0)        → All lights off
```
Mix them to make any color! This is how Photoshop, Figma, and every screen works.

### 2. Math.abs() — Finding the Difference
`Math.abs()` gives you the positive distance between two numbers (ignoring the sign):
```javascript
Math.abs(200 - 50)   // 150 (how far apart they are)
Math.abs(50 - 200)   // Also 150! Always positive
Math.abs(100 - 100)  // 0 (they match perfectly!)
```
We use this to measure how close your guess is to the target.

### 3. Game State — Tracking What's Happening
Games need to remember things: the current round, the score, what the target is. We use variables:
```javascript
let score = 0;           // Starts at zero
let currentRound = 1;    // Which round are we on?
let totalRounds = 5;     // How many rounds total?

// After each guess:
score = score + points;  // Add points
currentRound++;          // Move to next round (++ means +1)
```

### 4. parseInt() — Converting Text to Numbers
Sliders give you text like `"128"`. Math needs actual numbers:
```javascript
let textValue = "128";           // This is text (a string)
let number = parseInt("128");    // This is the number 128
number + 1;                      // 129 ✓ (math works!)
textValue + 1;                   // "1281" ✗ (joins text, not math!)
```

### 5. Conditional Logic — Making Decisions
`if/else` lets your code make decisions:
```javascript
if (matchPercent >= 90) {
    message = "Perfect!";
} else if (matchPercent >= 70) {
    message = "Good job!";
} else {
    message = "Keep trying!";
}
```

### How These Concepts Connect in This Project:
| Concept | How We Use It |
|---------|---------------|
| RGB Colors | Target color is random RGB; player mixes their own |
| Math.abs() | Calculate how far each R, G, B slider is from target |
| Game State | Track rounds, score, and difficulty |
| parseInt() | Convert slider values from text to numbers for math |
| Conditional Logic | Give different feedback based on match accuracy |

Now let's build! 👇

---

## 🛠️ Step-by-Step Instructions

### Step 1: Create the HTML
Create `index.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Color Match Game</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="game-container">
        <header class="game-header">
            <h1>🎨 Color Match Game</h1>
            <p class="subtitle">Mix RGB to match the target color!</p>
        </header>

        <!-- Score & Round -->
        <div class="game-info">
            <div class="info-box">
                <span class="info-label">Round</span>
                <span id="round" class="info-value">1 / 5</span>
            </div>
            <div class="info-box">
                <span class="info-label">Score</span>
                <span id="score" class="info-value">0</span>
            </div>
            <div class="info-box">
                <span class="info-label">Best</span>
                <span id="best-score" class="info-value">0</span>
            </div>
        </div>

        <!-- Color Display -->
        <div class="color-display">
            <div class="color-box">
                <div id="target-color" class="color-swatch"></div>
                <p class="color-label">Target</p>
            </div>
            <div class="vs-badge">VS</div>
            <div class="color-box">
                <div id="your-color" class="color-swatch"></div>
                <p class="color-label">Your Mix</p>
            </div>
        </div>

        <!-- RGB Sliders -->
        <div class="sliders">
            <div class="slider-group red">
                <label>
                    <span class="slider-icon">🔴</span> Red: <span id="red-value">128</span>
                </label>
                <input type="range" id="red-slider" min="0" max="255" value="128" class="color-slider red-slider">
            </div>

            <div class="slider-group green">
                <label>
                    <span class="slider-icon">🟢</span> Green: <span id="green-value">128</span>
                </label>
                <input type="range" id="green-slider" min="0" max="255" value="128" class="color-slider green-slider">
            </div>

            <div class="slider-group blue">
                <label>
                    <span class="slider-icon">🔵</span> Blue: <span id="blue-value">128</span>
                </label>
                <input type="range" id="blue-slider" min="0" max="255" value="128" class="color-slider blue-slider">
            </div>
        </div>

        <!-- Difficulty -->
        <div class="difficulty-selector">
            <label>Difficulty:</label>
            <button class="diff-btn active" data-difficulty="easy">Easy</button>
            <button class="diff-btn" data-difficulty="medium">Medium</button>
            <button class="diff-btn" data-difficulty="hard">Hard</button>
        </div>

        <!-- Action Buttons -->
        <div class="actions">
            <button id="submit-btn" class="btn submit-btn">✓ Submit Guess</button>
            <button id="hint-btn" class="btn hint-btn">💡 Hint</button>
            <button id="new-game-btn" class="btn new-game-btn">🔄 New Game</button>
        </div>

        <!-- Feedback -->
        <div id="feedback" class="feedback hidden"></div>

        <!-- Game Over Screen -->
        <div id="game-over" class="game-over hidden">
            <h2>🏆 Game Over!</h2>
            <p>Final Score: <span id="final-score">0</span> / <span id="max-score">500</span></p>
            <div id="grade" class="grade"></div>
            <button id="play-again-btn" class="btn submit-btn">Play Again</button>
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
    background: linear-gradient(135deg, #2c3e50, #3498db);
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 20px;
}

/* Game Container */
.game-container {
    background: white;
    border-radius: 20px;
    box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
    padding: 30px;
    max-width: 500px;
    width: 100%;
}

/* Header */
.game-header {
    text-align: center;
    margin-bottom: 20px;
}

.game-header h1 {
    font-size: 1.8rem;
    color: #2c3e50;
}

.subtitle {
    color: #666;
    font-size: 0.9rem;
    margin-top: 4px;
}

/* Game Info Bar */
.game-info {
    display: flex;
    justify-content: space-around;
    background: #f8f9fa;
    border-radius: 10px;
    padding: 12px;
    margin-bottom: 20px;
}

.info-box {
    text-align: center;
}

.info-label {
    display: block;
    font-size: 0.75rem;
    color: #888;
    text-transform: uppercase;
    letter-spacing: 1px;
}

.info-value {
    display: block;
    font-size: 1.3rem;
    font-weight: bold;
    color: #2c3e50;
}

/* Color Display */
.color-display {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 15px;
    margin-bottom: 25px;
}

.color-box {
    text-align: center;
}

.color-swatch {
    width: 120px;
    height: 120px;
    border-radius: 15px;
    border: 3px solid #eee;
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
    transition: background-color 0.1s ease;
}

.color-label {
    margin-top: 8px;
    font-size: 0.85rem;
    font-weight: 600;
    color: #555;
}

.vs-badge {
    font-weight: bold;
    font-size: 1.2rem;
    color: #999;
    background: #f0f0f0;
    width: 40px;
    height: 40px;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
}

/* Sliders */
.sliders {
    margin-bottom: 20px;
}

.slider-group {
    margin-bottom: 15px;
}

.slider-group label {
    display: block;
    font-size: 0.9rem;
    font-weight: 600;
    color: #333;
    margin-bottom: 6px;
}

.slider-icon {
    font-size: 0.8rem;
}

.color-slider {
    width: 100%;
    height: 10px;
    -webkit-appearance: none;
    appearance: none;
    border-radius: 5px;
    outline: none;
    cursor: pointer;
}

/* Red slider */
.red-slider {
    background: linear-gradient(to right, #000, #ff0000);
}

.red-slider::-webkit-slider-thumb {
    -webkit-appearance: none;
    appearance: none;
    width: 22px;
    height: 22px;
    border-radius: 50%;
    background: #ff4444;
    border: 3px solid white;
    box-shadow: 0 2px 6px rgba(0, 0, 0, 0.3);
}

/* Green slider */
.green-slider {
    background: linear-gradient(to right, #000, #00ff00);
}

.green-slider::-webkit-slider-thumb {
    -webkit-appearance: none;
    appearance: none;
    width: 22px;
    height: 22px;
    border-radius: 50%;
    background: #44cc44;
    border: 3px solid white;
    box-shadow: 0 2px 6px rgba(0, 0, 0, 0.3);
}

/* Blue slider */
.blue-slider {
    background: linear-gradient(to right, #000, #0000ff);
}

.blue-slider::-webkit-slider-thumb {
    -webkit-appearance: none;
    appearance: none;
    width: 22px;
    height: 22px;
    border-radius: 50%;
    background: #4444ff;
    border: 3px solid white;
    box-shadow: 0 2px 6px rgba(0, 0, 0, 0.3);
}

/* Difficulty */
.difficulty-selector {
    display: flex;
    align-items: center;
    gap: 8px;
    margin-bottom: 20px;
    justify-content: center;
}

.difficulty-selector label {
    font-size: 0.85rem;
    font-weight: 600;
    color: #555;
}

.diff-btn {
    padding: 6px 14px;
    border: 2px solid #ddd;
    border-radius: 20px;
    background: white;
    font-size: 0.8rem;
    cursor: pointer;
    transition: all 0.2s ease;
}

.diff-btn:hover {
    border-color: #3498db;
}

.diff-btn.active {
    background: #3498db;
    color: white;
    border-color: #3498db;
}

/* Action Buttons */
.actions {
    display: flex;
    gap: 10px;
    justify-content: center;
    flex-wrap: wrap;
}

.btn {
    padding: 12px 20px;
    border: none;
    border-radius: 10px;
    font-size: 0.9rem;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.2s ease;
}

.btn:hover {
    transform: translateY(-2px);
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
}

.submit-btn {
    background: linear-gradient(45deg, #27ae60, #2ecc71);
    color: white;
}

.hint-btn {
    background: linear-gradient(45deg, #f39c12, #e67e22);
    color: white;
}

.new-game-btn {
    background: linear-gradient(45deg, #3498db, #2980b9);
    color: white;
}

/* Feedback */
.feedback {
    text-align: center;
    margin-top: 20px;
    padding: 15px;
    border-radius: 10px;
    font-weight: 600;
    font-size: 1rem;
    animation: slideUp 0.3s ease;
}

.feedback.great {
    background: #d4efdf;
    color: #27ae60;
}

.feedback.good {
    background: #fef9e7;
    color: #f39c12;
}

.feedback.close {
    background: #fdebd0;
    color: #e67e22;
}

.feedback.far {
    background: #fadbd8;
    color: #e74c3c;
}

.feedback.hidden {
    display: none;
}

/* Game Over */
.game-over {
    text-align: center;
    padding: 25px;
    background: #f8f9fa;
    border-radius: 15px;
    margin-top: 20px;
    animation: slideUp 0.4s ease;
}

.game-over.hidden {
    display: none;
}

.game-over h2 {
    font-size: 1.5rem;
    margin-bottom: 10px;
    color: #2c3e50;
}

.game-over p {
    font-size: 1.1rem;
    color: #555;
    margin-bottom: 15px;
}

.grade {
    font-size: 2rem;
    margin-bottom: 15px;
}

/* Animations */
@keyframes slideUp {
    from {
        opacity: 0;
        transform: translateY(10px);
    }
    to {
        opacity: 1;
        transform: translateY(0);
    }
}

/* Responsive */
@media (max-width: 500px) {
    .game-container {
        padding: 20px;
    }

    .color-swatch {
        width: 90px;
        height: 90px;
    }

    .game-header h1 {
        font-size: 1.4rem;
    }

    .actions {
        flex-direction: column;
    }

    .btn {
        width: 100%;
    }
}
```

### Step 3: Add JavaScript (Game Logic!)
Create `script.js`:

```javascript
// ===== GAME STATE =====
let targetColor = { r: 0, g: 0, b: 0 };
let currentRound = 1;
let totalRounds = 5;
let score = 0;
let bestScore = 0;
let difficulty = "easy"; // easy, medium, hard
let hintsUsed = 0;

// ===== GET ELEMENTS =====
const targetSwatch = document.getElementById("target-color");
const yourSwatch = document.getElementById("your-color");
const redSlider = document.getElementById("red-slider");
const greenSlider = document.getElementById("green-slider");
const blueSlider = document.getElementById("blue-slider");
const redValue = document.getElementById("red-value");
const greenValue = document.getElementById("green-value");
const blueValue = document.getElementById("blue-value");
const roundDisplay = document.getElementById("round");
const scoreDisplay = document.getElementById("score");
const bestScoreDisplay = document.getElementById("best-score");
const submitBtn = document.getElementById("submit-btn");
const hintBtn = document.getElementById("hint-btn");
const newGameBtn = document.getElementById("new-game-btn");
const feedback = document.getElementById("feedback");
const gameOver = document.getElementById("game-over");
const finalScore = document.getElementById("final-score");
const maxScore = document.getElementById("max-score");
const gradeDisplay = document.getElementById("grade");
const playAgainBtn = document.getElementById("play-again-btn");
const diffButtons = document.querySelectorAll(".diff-btn");

// ===== GENERATE RANDOM COLOR =====
function generateTargetColor() {
    if (difficulty === "easy") {
        // Easy: colors in steps of 51 (0, 51, 102, 153, 204, 255)
        targetColor = {
            r: Math.round(Math.random() * 5) * 51,
            g: Math.round(Math.random() * 5) * 51,
            b: Math.round(Math.random() * 5) * 51
        };
    } else if (difficulty === "medium") {
        // Medium: steps of 17
        targetColor = {
            r: Math.round(Math.random() * 15) * 17,
            g: Math.round(Math.random() * 15) * 17,
            b: Math.round(Math.random() * 15) * 17
        };
    } else {
        // Hard: any value 0-255
        targetColor = {
            r: Math.floor(Math.random() * 256),
            g: Math.floor(Math.random() * 256),
            b: Math.floor(Math.random() * 256)
        };
    }

    // Show target color
    targetSwatch.style.backgroundColor = "rgb(" + targetColor.r + "," + targetColor.g + "," + targetColor.b + ")";
}

// ===== UPDATE YOUR COLOR (live as you move sliders) =====
function updateYourColor() {
    const r = redSlider.value;
    const g = greenSlider.value;
    const b = blueSlider.value;

    yourSwatch.style.backgroundColor = "rgb(" + r + "," + g + "," + b + ")";

    redValue.textContent = r;
    greenValue.textContent = g;
    blueValue.textContent = b;
}

// ===== CALCULATE MATCH SCORE =====
function calculateScore() {
    const r = parseInt(redSlider.value);
    const g = parseInt(greenSlider.value);
    const b = parseInt(blueSlider.value);

    // Calculate difference (0 = perfect match)
    const diffR = Math.abs(targetColor.r - r);
    const diffG = Math.abs(targetColor.g - g);
    const diffB = Math.abs(targetColor.b - b);

    // Total difference (max possible = 765)
    const totalDiff = diffR + diffG + diffB;

    // Convert to percentage match (0-100)
    const matchPercent = Math.round((1 - totalDiff / 765) * 100);

    // Score out of 100 per round
    let roundScore = matchPercent;

    // Penalty for hints
    roundScore = Math.max(0, roundScore - (hintsUsed * 10));

    return { roundScore: roundScore, matchPercent: matchPercent, totalDiff: totalDiff };
}

// ===== SHOW FEEDBACK =====
function showFeedback(matchPercent, roundScore) {
    feedback.classList.remove("hidden", "great", "good", "close", "far");

    let message = "";
    let feedbackClass = "";

    if (matchPercent >= 95) {
        message = "🎉 Perfect Eye! +" + roundScore + " points";
        feedbackClass = "great";
    } else if (matchPercent >= 80) {
        message = "👏 Great Match! +" + roundScore + " points";
        feedbackClass = "great";
    } else if (matchPercent >= 60) {
        message = "👍 Good Try! +" + roundScore + " points";
        feedbackClass = "good";
    } else if (matchPercent >= 40) {
        message = "🤔 Getting There! +" + roundScore + " points";
        feedbackClass = "close";
    } else {
        message = "😅 Keep Practicing! +" + roundScore + " points";
        feedbackClass = "far";
    }

    // Show the actual target values
    message += "<br><small>Target was: R:" + targetColor.r + " G:" + targetColor.g + " B:" + targetColor.b + "</small>";

    feedback.innerHTML = message;
    feedback.classList.add(feedbackClass);
}

// ===== SUBMIT GUESS =====
function submitGuess() {
    const result = calculateScore();

    // Add to score
    score += result.roundScore;
    scoreDisplay.textContent = score;

    // Show feedback
    showFeedback(result.matchPercent, result.roundScore);

    // Check if game is over
    if (currentRound >= totalRounds) {
        // Game over!
        setTimeout(showGameOver, 1500);
    } else {
        // Next round after short delay
        currentRound++;
        setTimeout(function () {
            startRound();
        }, 2000);
    }
}

// ===== START A NEW ROUND =====
function startRound() {
    // Reset
    hintsUsed = 0;
    feedback.classList.add("hidden");

    // Update display
    roundDisplay.textContent = currentRound + " / " + totalRounds;

    // Generate new target color
    generateTargetColor();

    // Reset sliders to middle
    redSlider.value = 128;
    greenSlider.value = 128;
    blueSlider.value = 128;
    updateYourColor();
}

// ===== SHOW GAME OVER =====
function showGameOver() {
    const maxPossible = totalRounds * 100;
    finalScore.textContent = score;
    maxScore.textContent = maxPossible;

    // Calculate grade
    const percent = (score / maxPossible) * 100;
    let grade = "";

    if (percent >= 90) {
        grade = "🏆 Master Designer!";
    } else if (percent >= 75) {
        grade = "🌟 Color Pro!";
    } else if (percent >= 60) {
        grade = "🎨 Good Eye!";
    } else if (percent >= 40) {
        grade = "👀 Keep Training!";
    } else {
        grade = "🔰 Beginner — Try Again!";
    }

    gradeDisplay.textContent = grade;
    gameOver.classList.remove("hidden");

    // Update best score
    if (score > bestScore) {
        bestScore = score;
        bestScoreDisplay.textContent = bestScore;
    }
}

// ===== HINT =====
function giveHint() {
    hintsUsed++;
    const r = targetColor.r;
    const g = targetColor.g;
    const b = targetColor.b;

    let hint = "";

    if (hintsUsed === 1) {
        // First hint: tell which channel is highest
        if (r >= g && r >= b) hint = "💡 Hint: Red is the strongest channel";
        else if (g >= r && g >= b) hint = "💡 Hint: Green is the strongest channel";
        else hint = "💡 Hint: Blue is the strongest channel";
    } else if (hintsUsed === 2) {
        // Second hint: tell if values are high or low
        hint = "💡 Hint: R is " + (r > 128 ? "above" : "below") + " 128, G is " + (g > 128 ? "above" : "below") + " 128, B is " + (b > 128 ? "above" : "below") + " 128";
    } else {
        // Third hint: give approximate values
        hint = "💡 Hint: ~R:" + (Math.round(r / 50) * 50) + " ~G:" + (Math.round(g / 50) * 50) + " ~B:" + (Math.round(b / 50) * 50);
    }

    feedback.classList.remove("hidden", "great", "good", "close", "far");
    feedback.innerHTML = hint + "<br><small>(-10 points per hint)</small>";
    feedback.classList.add("good");
}

// ===== NEW GAME =====
function newGame() {
    currentRound = 1;
    score = 0;
    scoreDisplay.textContent = 0;
    gameOver.classList.add("hidden");
    feedback.classList.add("hidden");
    startRound();
}

// ===== EVENT LISTENERS =====

// Sliders update live
redSlider.addEventListener("input", updateYourColor);
greenSlider.addEventListener("input", updateYourColor);
blueSlider.addEventListener("input", updateYourColor);

// Buttons
submitBtn.addEventListener("click", submitGuess);
hintBtn.addEventListener("click", giveHint);
newGameBtn.addEventListener("click", newGame);
playAgainBtn.addEventListener("click", newGame);

// Difficulty buttons
diffButtons.forEach(function (btn) {
    btn.addEventListener("click", function () {
        diffButtons.forEach(function (b) { b.classList.remove("active"); });
        btn.classList.add("active");
        difficulty = btn.getAttribute("data-difficulty");
        newGame();
    });
});

// ===== START GAME =====
startRound();
```

---

## 💡 How RGB Colors Work

Every color on your screen is made from **Red**, **Green**, and **Blue** light mixed together:

| Color | R | G | B |
|-------|---|---|---|
| Pure Red | 255 | 0 | 0 |
| Pure Green | 0 | 255 | 0 |
| Pure Blue | 0 | 0 | 255 |
| White | 255 | 255 | 255 |
| Black | 0 | 0 | 0 |
| Yellow | 255 | 255 | 0 |
| Cyan | 0 | 255 | 255 |
| Purple | 128 | 0 | 128 |
| Orange | 255 | 165 | 0 |

This is exactly how graphic designers think about color in tools like Photoshop, Figma, and Canva!

---

## 🎨 Creative Challenges

### Challenge 1: Add a Timer
Give players only 10 seconds per round:
```javascript
let timeLeft = 10;
let timer;

function startTimer() {
    timeLeft = 10;
    timer = setInterval(function() {
        timeLeft--;
        // Update display
        if (timeLeft <= 0) {
            clearInterval(timer);
            submitGuess(); // Auto-submit when time runs out
        }
    }, 1000);
}
```

### Challenge 2: HEX Code Mode
Show the target as a hex code instead of a color swatch (much harder!):
```javascript
function rgbToHex(r, g, b) {
    return "#" + r.toString(16).padStart(2, "0") + g.toString(16).padStart(2, "0") + b.toString(16).padStart(2, "0");
}
// Display: #FF6B6B instead of showing the color
```

### Challenge 3: HSL Mode
Add HSL sliders (Hue, Saturation, Lightness) as an alternative — this is how many designers actually think about color!

### Challenge 4: Two-Player Mode
Player 1 picks a color, Player 2 tries to match it. Take turns and compare scores!

### Challenge 5: Color Blind Friendly
Add a mode that shows the RGB numbers on the target swatch so color-blind players can still play.

---

## 🔍 What Did You Learn?
- How RGB color mixing works (essential for graphic design!)
- Game logic: rounds, scoring, win conditions
- Real-time UI updates with sliders
- Giving feedback to users
- Difficulty scaling
- Math for comparing values

## 🎓 Why This Matters for Design
Professional designers need to:
- Identify colors by eye
- Understand RGB, HEX, and HSL values
- Match brand colors accurately
- Choose complementary colors

This game literally trains the same skill! Try playing on "Hard" mode to really test your eye. 🎯
