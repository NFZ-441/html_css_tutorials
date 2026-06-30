# Tutorial 5: Zodiac & Personality Card Generator ✨

## 🎯 Objective
Build a beautiful card generator where you pick your zodiac sign (or answer personality questions) and get a stunning, personalized card with your traits, colors, element, and symbol. Share-worthy results every time!

## 📚 What You'll Learn
- JavaScript objects and arrays (storing zodiac data)
- Conditional logic (showing different results)
- Dynamic styling (changing colors/gradients with code)
- CSS animations for card reveals
- Building a multi-step UI (question flow)

## 🖥️ How to Run This
**Option A:** Create files in vscode.dev and use Live Preview
**Option B:** Paste all the code into [CodePen.io](https://codepen.io)

---

## 📖 Concepts You'll Use (Read This First!)

### 1. Objects — Storing Structured Data
An object holds related information together using key-value pairs. Think of it like a profile card in code:
```javascript
let aries = {
    name: "Aries",
    symbol: "♈",
    traits: ["Bold", "Brave", "Energetic"],
    luckyNumber: 9
};

// Read values using dot notation:
aries.name          // "Aries"
aries.traits[0]     // "Bold" (first item in the list)
aries.luckyNumber   // 9
```
We store ALL zodiac data this way — 12 objects with traits, colors, quotes, and more.

### 2. Arrays — Lists of Things
An array is an ordered list. You can loop through it or pick items by position:
```javascript
let colors = ["red", "blue", "green"];

colors[0]        // "red" (first item — counting starts at 0!)
colors[2]        // "green" (third item)
colors.length    // 3 (how many items)

// Add to the end:
colors.push("yellow");  // Now: ["red", "blue", "green", "yellow"]
```

### 3. Multi-Screen Navigation — Showing One Screen at a Time
Instead of multiple HTML pages, we put all screens in one page and show/hide them:
```javascript
// Hide all screens
allScreens.forEach(function(screen) {
    screen.classList.remove("active");  // Hidden via CSS
});

// Show the one we want
targetScreen.classList.add("active");   // Visible via CSS
```
```css
.screen { display: none; }         /* Hidden by default */
.screen.active { display: block; } /* Visible when active */
```

### 4. createElement() — Building HTML with Code
Instead of writing every element in HTML, JavaScript can create them dynamically:
```javascript
let tag = document.createElement("span");  // Create a <span>
tag.textContent = "Bold";                  // Put text in it
tag.classList.add("trait-tag");             // Add a CSS class
container.appendChild(tag);                // Add it to the page
```
This is how we build trait tags — one per trait in the array.

### 5. Counting & Comparing — Quiz Scoring
The quiz counts which zodiac sign appears most across all answers:
```javascript
let signCount = {};  // Empty object to tally results

// After each answer, add to the count:
signCount["aries"] = (signCount["aries"] || 0) + 1;

// Find the winner — whoever has the highest count:
// aries: 3, leo: 2, gemini: 1 → Result: Aries!
```

### How These Concepts Connect in This Project:
| Concept | How We Use It |
|---------|---------------|
| Objects | Store all 12 zodiac signs' data (traits, colors, quotes) |
| Arrays | Hold lists of traits, quiz answers, gradient colors |
| Multi-Screen Navigation | Flow from Start → Quiz/Picker → Result Card |
| createElement() | Dynamically build trait tags on the result card |
| Counting & Comparing | Tally quiz answers to determine your zodiac match |

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
    <title>Zodiac Card Generator</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="app-container">
        <!-- Start Screen -->
        <div id="start-screen" class="screen active">
            <div class="stars-bg"></div>
            <h1 class="main-title">✨ Zodiac Card Generator ✨</h1>
            <p class="intro-text">Discover your cosmic identity</p>

            <div class="mode-select">
                <button id="zodiac-mode-btn" class="mode-btn">
                    <span class="mode-icon">♈</span>
                    <span class="mode-label">Pick My Sign</span>
                    <span class="mode-desc">Choose your zodiac sign</span>
                </button>
                <button id="quiz-mode-btn" class="mode-btn">
                    <span class="mode-icon">🔮</span>
                    <span class="mode-label">Personality Quiz</span>
                    <span class="mode-desc">Answer 3 questions</span>
                </button>
            </div>
        </div>

        <!-- Zodiac Picker Screen -->
        <div id="zodiac-screen" class="screen">
            <h2>Choose Your Zodiac Sign</h2>
            <div class="zodiac-grid">
                <button class="zodiac-btn" data-sign="aries">♈ Aries<br><small>Mar 21 - Apr 19</small></button>
                <button class="zodiac-btn" data-sign="taurus">♉ Taurus<br><small>Apr 20 - May 20</small></button>
                <button class="zodiac-btn" data-sign="gemini">♊ Gemini<br><small>May 21 - Jun 20</small></button>
                <button class="zodiac-btn" data-sign="cancer">♋ Cancer<br><small>Jun 21 - Jul 22</small></button>
                <button class="zodiac-btn" data-sign="leo">♌ Leo<br><small>Jul 23 - Aug 22</small></button>
                <button class="zodiac-btn" data-sign="virgo">♍ Virgo<br><small>Aug 23 - Sep 22</small></button>
                <button class="zodiac-btn" data-sign="libra">♎ Libra<br><small>Sep 23 - Oct 22</small></button>
                <button class="zodiac-btn" data-sign="scorpio">♏ Scorpio<br><small>Oct 23 - Nov 21</small></button>
                <button class="zodiac-btn" data-sign="sagittarius">♐ Sagittarius<br><small>Nov 22 - Dec 21</small></button>
                <button class="zodiac-btn" data-sign="capricorn">♑ Capricorn<br><small>Dec 22 - Jan 19</small></button>
                <button class="zodiac-btn" data-sign="aquarius">♒ Aquarius<br><small>Jan 20 - Feb 18</small></button>
                <button class="zodiac-btn" data-sign="pisces">♓ Pisces<br><small>Feb 19 - Mar 20</small></button>
            </div>
            <button id="back-btn-zodiac" class="back-btn">← Back</button>
        </div>

        <!-- Quiz Screen -->
        <div id="quiz-screen" class="screen">
            <div class="quiz-progress">
                <div id="progress-bar" class="progress-bar" style="width: 33%"></div>
            </div>
            <h2 id="question-title">Question 1 of 3</h2>
            <p id="question-text" class="question-text"></p>
            <div id="quiz-options" class="quiz-options"></div>
            <button id="back-btn-quiz" class="back-btn">← Back</button>
        </div>

        <!-- Result Card Screen -->
        <div id="result-screen" class="screen">
            <div id="zodiac-card" class="zodiac-card">
                <div class="card-glow"></div>
                <div class="card-content">
                    <div id="card-symbol" class="card-symbol">♈</div>
                    <h2 id="card-name" class="card-name">Aries</h2>
                    <p id="card-dates" class="card-dates">March 21 - April 19</p>
                    <div id="card-element" class="card-element">🔥 Fire</div>
                    <div class="card-divider"></div>
                    <div class="traits-section">
                        <h3>Traits</h3>
                        <div id="card-traits" class="card-traits"></div>
                    </div>
                    <div class="card-divider"></div>
                    <div class="extras">
                        <div class="extra-item">
                            <span class="extra-label">Lucky Number</span>
                            <span id="card-number" class="extra-value">7</span>
                        </div>
                        <div class="extra-item">
                            <span class="extra-label">Spirit Animal</span>
                            <span id="card-animal" class="extra-value">🐺</span>
                        </div>
                        <div class="extra-item">
                            <span class="extra-label">Power Color</span>
                            <span id="card-power-color" class="extra-value">Red</span>
                        </div>
                    </div>
                    <p id="card-quote" class="card-quote">"Adventure awaits the bold."</p>
                </div>
            </div>
            <div class="result-actions">
                <button id="regenerate-btn" class="action-btn">🔄 Try Another</button>
                <button id="share-btn" class="action-btn share">📸 Screenshot This!</button>
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
    background: #0a0a1a;
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 20px;
    overflow-x: hidden;
}

/* App Container */
.app-container {
    width: 100%;
    max-width: 550px;
}

/* Screens */
.screen {
    display: none;
    animation: fadeIn 0.5s ease;
}

.screen.active {
    display: block;
}

@keyframes fadeIn {
    from { opacity: 0; transform: translateY(20px); }
    to { opacity: 1; transform: translateY(0); }
}

/* Start Screen */
.main-title {
    text-align: center;
    font-size: 2.2rem;
    color: white;
    margin-bottom: 10px;
    text-shadow: 0 0 20px rgba(147, 112, 219, 0.5);
}

.intro-text {
    text-align: center;
    color: #aaa;
    font-size: 1.1rem;
    margin-bottom: 30px;
}

/* Mode Selection */
.mode-select {
    display: flex;
    gap: 15px;
    justify-content: center;
    flex-wrap: wrap;
}

.mode-btn {
    background: rgba(255, 255, 255, 0.05);
    border: 2px solid rgba(255, 255, 255, 0.15);
    border-radius: 16px;
    padding: 25px 20px;
    cursor: pointer;
    transition: all 0.3s ease;
    text-align: center;
    width: 200px;
    color: white;
}

.mode-btn:hover {
    background: rgba(147, 112, 219, 0.2);
    border-color: rgba(147, 112, 219, 0.6);
    transform: translateY(-5px);
    box-shadow: 0 10px 30px rgba(147, 112, 219, 0.3);
}

.mode-icon {
    display: block;
    font-size: 2.5rem;
    margin-bottom: 10px;
}

.mode-label {
    display: block;
    font-size: 1rem;
    font-weight: 600;
    margin-bottom: 5px;
}

.mode-desc {
    display: block;
    font-size: 0.8rem;
    opacity: 0.6;
}

/* Zodiac Grid */
.zodiac-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 10px;
    margin: 20px 0;
}

.zodiac-btn {
    background: rgba(255, 255, 255, 0.05);
    border: 2px solid rgba(255, 255, 255, 0.1);
    border-radius: 12px;
    padding: 15px 10px;
    color: white;
    font-size: 1rem;
    cursor: pointer;
    transition: all 0.3s ease;
}

.zodiac-btn:hover {
    background: rgba(147, 112, 219, 0.3);
    border-color: rgba(147, 112, 219, 0.7);
    transform: scale(1.05);
}

.zodiac-btn small {
    opacity: 0.6;
    font-size: 0.7rem;
}

/* Quiz Screen */
.quiz-progress {
    height: 6px;
    background: rgba(255, 255, 255, 0.1);
    border-radius: 3px;
    margin-bottom: 25px;
    overflow: hidden;
}

.progress-bar {
    height: 100%;
    background: linear-gradient(90deg, #9370db, #ff6b9d);
    border-radius: 3px;
    transition: width 0.4s ease;
}

#quiz-screen h2 {
    color: white;
    text-align: center;
    margin-bottom: 10px;
}

.question-text {
    color: #ccc;
    text-align: center;
    font-size: 1.1rem;
    margin-bottom: 20px;
}

.quiz-options {
    display: flex;
    flex-direction: column;
    gap: 10px;
}

.quiz-option {
    background: rgba(255, 255, 255, 0.05);
    border: 2px solid rgba(255, 255, 255, 0.15);
    border-radius: 12px;
    padding: 15px 20px;
    color: white;
    font-size: 1rem;
    cursor: pointer;
    transition: all 0.2s ease;
    text-align: left;
}

.quiz-option:hover {
    background: rgba(147, 112, 219, 0.2);
    border-color: rgba(147, 112, 219, 0.6);
    transform: translateX(5px);
}

/* Result Card */
.zodiac-card {
    position: relative;
    background: linear-gradient(135deg, #1a1a3e, #2d1b69);
    border-radius: 24px;
    padding: 40px 30px;
    text-align: center;
    color: white;
    overflow: hidden;
    box-shadow: 0 20px 60px rgba(0, 0, 0, 0.5);
    animation: cardReveal 0.8s ease;
}

@keyframes cardReveal {
    from {
        opacity: 0;
        transform: scale(0.8) rotateY(10deg);
    }
    to {
        opacity: 1;
        transform: scale(1) rotateY(0);
    }
}

.card-glow {
    position: absolute;
    top: -50%;
    left: -50%;
    width: 200%;
    height: 200%;
    background: radial-gradient(circle, rgba(147, 112, 219, 0.15) 0%, transparent 70%);
    animation: rotate 10s linear infinite;
}

@keyframes rotate {
    from { transform: rotate(0deg); }
    to { transform: rotate(360deg); }
}

.card-content {
    position: relative;
    z-index: 2;
}

.card-symbol {
    font-size: 4rem;
    margin-bottom: 10px;
    animation: float 3s ease-in-out infinite;
}

@keyframes float {
    0%, 100% { transform: translateY(0); }
    50% { transform: translateY(-10px); }
}

.card-name {
    font-size: 2rem;
    font-weight: 700;
    margin-bottom: 5px;
    letter-spacing: 2px;
}

.card-dates {
    opacity: 0.7;
    font-size: 0.9rem;
    margin-bottom: 15px;
}

.card-element {
    display: inline-block;
    background: rgba(255, 255, 255, 0.1);
    padding: 6px 16px;
    border-radius: 20px;
    font-size: 0.9rem;
    margin-bottom: 20px;
}

.card-divider {
    height: 1px;
    background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.3), transparent);
    margin: 15px 0;
}

/* Traits */
.traits-section h3 {
    font-size: 0.85rem;
    text-transform: uppercase;
    letter-spacing: 2px;
    opacity: 0.6;
    margin-bottom: 10px;
}

.card-traits {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
    justify-content: center;
}

.trait-tag {
    background: rgba(255, 255, 255, 0.1);
    border: 1px solid rgba(255, 255, 255, 0.2);
    padding: 5px 12px;
    border-radius: 15px;
    font-size: 0.85rem;
}

/* Extras */
.extras {
    display: flex;
    justify-content: space-around;
    margin: 15px 0;
}

.extra-item {
    text-align: center;
}

.extra-label {
    display: block;
    font-size: 0.7rem;
    text-transform: uppercase;
    letter-spacing: 1px;
    opacity: 0.5;
    margin-bottom: 4px;
}

.extra-value {
    display: block;
    font-size: 1.2rem;
    font-weight: 600;
}

/* Quote */
.card-quote {
    font-style: italic;
    opacity: 0.7;
    font-size: 0.9rem;
    margin-top: 15px;
}

/* Action buttons */
.result-actions {
    display: flex;
    gap: 10px;
    justify-content: center;
    margin-top: 20px;
}

.action-btn {
    padding: 12px 20px;
    border: 2px solid rgba(255, 255, 255, 0.2);
    border-radius: 10px;
    background: rgba(255, 255, 255, 0.05);
    color: white;
    font-size: 0.9rem;
    cursor: pointer;
    transition: all 0.2s ease;
}

.action-btn:hover {
    background: rgba(255, 255, 255, 0.15);
    transform: translateY(-2px);
}

.action-btn.share {
    background: rgba(147, 112, 219, 0.3);
    border-color: rgba(147, 112, 219, 0.6);
}

/* Back button */
.back-btn {
    display: block;
    margin: 20px auto 0;
    padding: 10px 20px;
    background: none;
    border: 1px solid rgba(255, 255, 255, 0.2);
    border-radius: 8px;
    color: #aaa;
    cursor: pointer;
    font-size: 0.85rem;
    transition: all 0.2s ease;
}

.back-btn:hover {
    border-color: white;
    color: white;
}

/* Headings for screens */
#zodiac-screen h2 {
    color: white;
    text-align: center;
    margin-bottom: 15px;
}

/* Responsive */
@media (max-width: 500px) {
    .zodiac-grid {
        grid-template-columns: repeat(2, 1fr);
    }

    .main-title {
        font-size: 1.6rem;
    }

    .mode-select {
        flex-direction: column;
        align-items: center;
    }

    .mode-btn {
        width: 100%;
    }

    .extras {
        flex-direction: column;
        gap: 10px;
    }

    .result-actions {
        flex-direction: column;
    }
}
```

### Step 3: Add JavaScript
Create `script.js`:

```javascript
// ===== ZODIAC DATA =====
const zodiacData = {
    aries: {
        name: "Aries",
        symbol: "♈",
        dates: "March 21 - April 19",
        element: "🔥 Fire",
        traits: ["Bold", "Ambitious", "Energetic", "Honest", "Adventurous"],
        luckyNumber: 9,
        animal: "🐏",
        powerColor: "Red",
        quote: "\"The stars reward those who dare.\"",
        gradient: ["#ff416c", "#ff4b2b"]
    },
    taurus: {
        name: "Taurus",
        symbol: "♉",
        dates: "April 20 - May 20",
        element: "🌍 Earth",
        traits: ["Reliable", "Patient", "Devoted", "Strong", "Creative"],
        luckyNumber: 6,
        animal: "🐂",
        powerColor: "Green",
        quote: "\"Steady hands build lasting worlds.\"",
        gradient: ["#56ab2f", "#a8e063"]
    },
    gemini: {
        name: "Gemini",
        symbol: "♊",
        dates: "May 21 - June 20",
        element: "💨 Air",
        traits: ["Curious", "Adaptable", "Witty", "Social", "Clever"],
        luckyNumber: 5,
        animal: "🦋",
        powerColor: "Yellow",
        quote: "\"Two minds are better than one.\"",
        gradient: ["#f7971e", "#ffd200"]
    },
    cancer: {
        name: "Cancer",
        symbol: "♋",
        dates: "June 21 - July 22",
        element: "💧 Water",
        traits: ["Loyal", "Caring", "Intuitive", "Protective", "Creative"],
        luckyNumber: 2,
        animal: "🦀",
        powerColor: "Silver",
        quote: "\"Home is where the heart finds peace.\"",
        gradient: ["#667eea", "#764ba2"]
    },
    leo: {
        name: "Leo",
        symbol: "♌",
        dates: "July 23 - August 22",
        element: "🔥 Fire",
        traits: ["Confident", "Dramatic", "Generous", "Leader", "Warm"],
        luckyNumber: 1,
        animal: "🦁",
        powerColor: "Gold",
        quote: "\"Born to shine, destined to lead.\"",
        gradient: ["#f12711", "#f5af19"]
    },
    virgo: {
        name: "Virgo",
        symbol: "♍",
        dates: "August 23 - September 22",
        element: "🌍 Earth",
        traits: ["Analytical", "Kind", "Hardworking", "Practical", "Detail-focused"],
        luckyNumber: 5,
        animal: "🦊",
        powerColor: "Navy",
        quote: "\"Perfection is found in the details.\"",
        gradient: ["#2c3e50", "#4ca1af"]
    },
    libra: {
        name: "Libra",
        symbol: "♎",
        dates: "September 23 - October 22",
        element: "💨 Air",
        traits: ["Balanced", "Fair", "Social", "Artistic", "Peaceful"],
        luckyNumber: 7,
        animal: "🕊️",
        powerColor: "Pink",
        quote: "\"Harmony is the ultimate art.\"",
        gradient: ["#ee9ca7", "#ffdde1"]
    },
    scorpio: {
        name: "Scorpio",
        symbol: "♏",
        dates: "October 23 - November 21",
        element: "💧 Water",
        traits: ["Passionate", "Brave", "Mysterious", "Focused", "Loyal"],
        luckyNumber: 8,
        animal: "🦅",
        powerColor: "Black",
        quote: "\"Still waters run deep.\"",
        gradient: ["#200122", "#6f0000"]
    },
    sagittarius: {
        name: "Sagittarius",
        symbol: "♐",
        dates: "November 22 - December 21",
        element: "🔥 Fire",
        traits: ["Optimistic", "Adventurous", "Funny", "Free-spirited", "Honest"],
        luckyNumber: 3,
        animal: "🐎",
        powerColor: "Purple",
        quote: "\"The world is a book; travel every page.\"",
        gradient: ["#6a3093", "#a044ff"]
    },
    capricorn: {
        name: "Capricorn",
        symbol: "♑",
        dates: "December 22 - January 19",
        element: "🌍 Earth",
        traits: ["Disciplined", "Responsible", "Ambitious", "Wise", "Patient"],
        luckyNumber: 4,
        animal: "🐐",
        powerColor: "Brown",
        quote: "\"The climb is the reward.\"",
        gradient: ["#2c3e50", "#bdc3c7"]
    },
    aquarius: {
        name: "Aquarius",
        symbol: "♒",
        dates: "January 20 - February 18",
        element: "💨 Air",
        traits: ["Innovative", "Independent", "Unique", "Visionary", "Humanitarian"],
        luckyNumber: 11,
        animal: "🦄",
        powerColor: "Electric Blue",
        quote: "\"The future belongs to the dreamers.\"",
        gradient: ["#00c6ff", "#0072ff"]
    },
    pisces: {
        name: "Pisces",
        symbol: "♓",
        dates: "February 19 - March 20",
        element: "💧 Water",
        traits: ["Compassionate", "Artistic", "Intuitive", "Dreamy", "Gentle"],
        luckyNumber: 7,
        animal: "🐬",
        powerColor: "Sea Green",
        quote: "\"Imagination is the only limit.\"",
        gradient: ["#36d1dc", "#5b86e5"]
    }
};

// ===== QUIZ DATA =====
const quizQuestions = [
    {
        question: "It's a free weekend. What are you doing?",
        options: [
            { text: "🏃 Something active — sports, hiking, exploring", signs: ["aries", "sagittarius", "leo"] },
            { text: "🎨 Creating something — art, music, writing", signs: ["pisces", "libra", "cancer"] },
            { text: "📚 Learning something new or organizing", signs: ["virgo", "capricorn", "aquarius"] },
            { text: "👯 Hanging with friends or meeting new people", signs: ["gemini", "taurus", "scorpio"] }
        ]
    },
    {
        question: "Pick the vibe that feels most like you:",
        options: [
            { text: "🔥 Intense and passionate — all or nothing", signs: ["scorpio", "aries", "leo"] },
            { text: "🌊 Chill and go-with-the-flow", signs: ["pisces", "taurus", "libra"] },
            { text: "⚡ Energetic and always on the move", signs: ["gemini", "sagittarius", "aquarius"] },
            { text: "🌙 Quiet strength — deep thinker", signs: ["capricorn", "virgo", "cancer"] }
        ]
    },
    {
        question: "Your friend group would describe you as:",
        options: [
            { text: "🎯 The leader — you make things happen", signs: ["aries", "leo", "capricorn"] },
            { text: "💝 The caring one — always checking on people", signs: ["cancer", "pisces", "taurus"] },
            { text: "🧠 The smart one — full of ideas and facts", signs: ["virgo", "aquarius", "gemini"] },
            { text: "🎭 The fun one — always entertaining", signs: ["sagittarius", "libra", "scorpio"] }
        ]
    }
];

// ===== GAME STATE =====
let currentQuestion = 0;
let quizAnswers = [];

// ===== GET ELEMENTS =====
const screens = {
    start: document.getElementById("start-screen"),
    zodiac: document.getElementById("zodiac-screen"),
    quiz: document.getElementById("quiz-screen"),
    result: document.getElementById("result-screen")
};

// ===== SCREEN NAVIGATION =====
function showScreen(screenName) {
    // Hide all screens
    Object.values(screens).forEach(function (screen) {
        screen.classList.remove("active");
    });
    // Show target screen
    screens[screenName].classList.add("active");
}

// ===== MODE BUTTONS =====
document.getElementById("zodiac-mode-btn").addEventListener("click", function () {
    showScreen("zodiac");
});

document.getElementById("quiz-mode-btn").addEventListener("click", function () {
    currentQuestion = 0;
    quizAnswers = [];
    showQuestion();
    showScreen("quiz");
});

// ===== BACK BUTTONS =====
document.getElementById("back-btn-zodiac").addEventListener("click", function () {
    showScreen("start");
});

document.getElementById("back-btn-quiz").addEventListener("click", function () {
    showScreen("start");
});

document.getElementById("regenerate-btn").addEventListener("click", function () {
    showScreen("start");
});

// ===== ZODIAC PICKER =====
const zodiacButtons = document.querySelectorAll(".zodiac-btn");
zodiacButtons.forEach(function (btn) {
    btn.addEventListener("click", function () {
        const sign = btn.getAttribute("data-sign");
        showResult(sign);
    });
});

// ===== QUIZ LOGIC =====
function showQuestion() {
    const q = quizQuestions[currentQuestion];
    document.getElementById("question-title").textContent = "Question " + (currentQuestion + 1) + " of 3";
    document.getElementById("question-text").textContent = q.question;
    document.getElementById("progress-bar").style.width = ((currentQuestion + 1) / 3 * 100) + "%";

    const optionsContainer = document.getElementById("quiz-options");
    optionsContainer.innerHTML = "";

    q.options.forEach(function (option) {
        const btn = document.createElement("button");
        btn.classList.add("quiz-option");
        btn.textContent = option.text;
        btn.addEventListener("click", function () {
            quizAnswers.push(option.signs);
            currentQuestion++;

            if (currentQuestion >= 3) {
                // Calculate result
                const result = calculateQuizResult();
                showResult(result);
            } else {
                showQuestion();
            }
        });
        optionsContainer.appendChild(btn);
    });
}

function calculateQuizResult() {
    // Count how many times each sign appeared
    const signCount = {};

    quizAnswers.forEach(function (signs) {
        signs.forEach(function (sign) {
            if (signCount[sign]) {
                signCount[sign]++;
            } else {
                signCount[sign] = 1;
            }
        });
    });

    // Find the sign with highest count
    let bestSign = "aries";
    let bestCount = 0;

    Object.keys(signCount).forEach(function (sign) {
        if (signCount[sign] > bestCount) {
            bestCount = signCount[sign];
            bestSign = sign;
        }
    });

    return bestSign;
}

// ===== SHOW RESULT CARD =====
function showResult(sign) {
    const data = zodiacData[sign];

    // Update card content
    document.getElementById("card-symbol").textContent = data.symbol;
    document.getElementById("card-name").textContent = data.name;
    document.getElementById("card-dates").textContent = data.dates;
    document.getElementById("card-element").textContent = data.element;
    document.getElementById("card-number").textContent = data.luckyNumber;
    document.getElementById("card-animal").textContent = data.animal;
    document.getElementById("card-power-color").textContent = data.powerColor;
    document.getElementById("card-quote").textContent = data.quote;

    // Update traits
    const traitsContainer = document.getElementById("card-traits");
    traitsContainer.innerHTML = "";
    data.traits.forEach(function (trait) {
        const tag = document.createElement("span");
        tag.classList.add("trait-tag");
        tag.textContent = trait;
        traitsContainer.appendChild(tag);
    });

    // Update card gradient
    const card = document.getElementById("zodiac-card");
    card.style.background = "linear-gradient(135deg, " + data.gradient[0] + ", " + data.gradient[1] + ")";

    // Show result screen
    showScreen("result");
}

// ===== SHARE BUTTON (screenshot reminder) =====
document.getElementById("share-btn").addEventListener("click", function () {
    alert("📸 Take a screenshot to share your card!\n\nOn Chromebook: Press Ctrl + Show Windows key\nor Ctrl + Shift + Show Windows to select an area.");
});
```

---

## 💡 Understanding the Code

### JavaScript Objects (Storing Zodiac Data):
```javascript
// An object stores related information together
const aries = {
    name: "Aries",        // text
    luckyNumber: 9,       // number
    traits: ["Bold", "Brave"]  // array (list)
};

// Access with dot notation:
aries.name       // "Aries"
aries.traits[0]  // "Bold"
```

### Showing/Hiding Screens:
```javascript
// Remove "active" class = hidden
// Add "active" class = visible
screen.classList.remove("active");  // hide
screen.classList.add("active");     // show
```

### Quiz Scoring Logic:
Each answer links to 3 possible signs. After 3 questions, we count which sign appeared most — that's your result!

| Concept | What It Does |
|---------|--------------|
| Objects `{}` | Store zodiac data with key-value pairs |
| Arrays `[]` | Store lists of traits, colors, quiz answers |
| `forEach()` | Loop through every item in an array |
| `classList` | Add/remove CSS classes to show/hide things |
| `createElement()` | Build new HTML elements with code |
| `innerHTML` | Replace the content inside an element |

---

## 🎨 Creative Challenges

### Challenge 1: Add Your Own Zodiac Descriptions
Customize the traits, quotes, and spirit animals to be funnier or more personal to your friend group!

### Challenge 2: Animated Stars Background
Add twinkling stars behind the card:
```css
.stars-bg {
    position: fixed;
    top: 0; left: 0;
    width: 100%; height: 100%;
    background-image: 
        radial-gradient(2px 2px at 20px 30px, white, transparent),
        radial-gradient(2px 2px at 40px 70px, white, transparent),
        radial-gradient(1px 1px at 90px 40px, white, transparent),
        radial-gradient(1px 1px at 130px 80px, white, transparent);
    background-size: 200px 100px;
    animation: twinkle 4s ease-in-out infinite alternate;
}

@keyframes twinkle {
    from { opacity: 0.3; }
    to { opacity: 1; }
}
```

### Challenge 3: Compatibility Checker
Add a section where two people pick their signs and see if they're compatible:
```javascript
const compatibility = {
    aries: ["leo", "sagittarius", "gemini"],
    taurus: ["virgo", "capricorn", "cancer"],
    // ... etc
};
```

### Challenge 4: More Quiz Questions
Add questions 4 and 5 for more accurate results. Ideas:
- "Pick a superpower..."
- "Choose a season..."
- "Pick a snack..."

### Challenge 5: Daily Horoscope
Add a random "daily message" that changes each day:
```javascript
const dailyMessages = [
    "Today is great for starting something new!",
    "Trust your instincts today.",
    "A surprise is coming your way...",
    "Focus on what makes you happy."
];
const today = new Date().getDay();
const todayMessage = dailyMessages[today % dailyMessages.length];
```

---

## 🔍 What Did You Learn?
- JavaScript objects for structured data
- Multi-screen app navigation
- Quiz logic and scoring algorithms
- Dynamic CSS (changing styles with JavaScript)
- Creating elements programmatically
- Array methods (forEach, push)
- Building a complete interactive experience

## ✨ Why Students Love This
- Everyone wants to see THEIR card
- Results are personal and shareable
- The personality quiz is inherently social
- Beautiful card design feels professional
- They'll customize it with inside jokes and friend group references

Screenshot your card and share it with the class! 📸
