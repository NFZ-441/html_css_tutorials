# Tutorial 1: Personal Profile Card

## 🎯 Objective
Create a personal profile card that displays your information in an attractive, card-style layout. You'll learn HTML structure basics and fundamental CSS styling.

## 📚 What You'll Learn
- HTML document structure
- Basic HTML tags (div, h1, p, img)
- CSS selectors and properties
- Box model (padding, margin, border)
- Colors and typography

## 🛠️ Step-by-Step Instructions

### Step 1: Create the HTML Structure
Create a file called `index.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Profile Card</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="profile-card">
        <div class="profile-header">
            <img src="https://via.placeholder.com/150" alt="Profile Picture" class="profile-image">
            <h1 class="profile-name">Your Name</h1>
            <p class="profile-title">Year 10 Student</p>
        </div>
        
        <div class="profile-info">
            <div class="info-item">
                <span class="info-label">Age:</span>
                <span class="info-value">15</span>
            </div>
            
            <div class="info-item">
                <span class="info-label">Favorite Subject:</span>
                <span class="info-value">Computer Science</span>
            </div>
            
            <div class="info-item">
                <span class="info-label">Hobby:</span>
                <span class="info-value">Gaming & Coding</span>
            </div>
            
            <div class="info-item">
                <span class="info-label">Dream Job:</span>
                <span class="info-value">Software Developer</span>
            </div>
        </div>
        
        <div class="profile-footer">
            <p class="profile-quote">"Learning to code is learning to create!"</p>
        </div>
    </div>
</body>
</html>
```

### Step 2: Add CSS Styling
Create a file called `style.css`:

```css
/* Reset and basic styling */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: 'Arial', sans-serif;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 20px;
}

/* Profile card container */
.profile-card {
    background: white;
    border-radius: 20px;
    box-shadow: 0 20px 40px rgba(0, 0, 0, 0.1);
    overflow: hidden;
    width: 100%;
    max-width: 400px;
    transition: transform 0.3s ease;
}

.profile-card:hover {
    transform: translateY(-5px);
}

/* Profile header section */
.profile-header {
    background: linear-gradient(135deg, #ff6b6b, #ffd93d);
    color: white;
    text-align: center;
    padding: 30px 20px;
}

.profile-image {
    width: 120px;
    height: 120px;
    border-radius: 50%;
    border: 4px solid white;
    margin-bottom: 15px;
    object-fit: cover;
}

.profile-name {
    font-size: 28px;
    margin-bottom: 5px;
    text-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
}

.profile-title {
    font-size: 16px;
    opacity: 0.9;
}

/* Profile info section */
.profile-info {
    padding: 25px;
}

.info-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 12px 0;
    border-bottom: 1px solid #f0f0f0;
}

.info-item:last-child {
    border-bottom: none;
}

.info-label {
    font-weight: bold;
    color: #555;
}

.info-value {
    color: #333;
    font-weight: 500;
}

/* Profile footer */
.profile-footer {
    background: #f8f9fa;
    padding: 20px;
    text-align: center;
}

.profile-quote {
    font-style: italic;
    color: #666;
    font-size: 14px;
}
```

### Step 3: Customize Your Card
1. Replace "Your Name" with your actual name
2. Update all the information with your details
3. Change the profile image URL to your photo (or keep the placeholder)
4. Modify the quote to something that represents you

## 💡 Tips & Tricks
- Use `border-radius` to make rounded corners
- The `box-shadow` property creates depth and makes elements "float"
- `linear-gradient` creates smooth color transitions
- The `:hover` pseudo-class adds interactivity

## 🎨 Creative Challenges

### Challenge 1: Color Themes
Create different color themes by changing the gradient colors:
```css
/* Dark theme */
background: linear-gradient(135deg, #2c3e50, #34495e);

/* Ocean theme */
background: linear-gradient(135deg, #3498db, #2980b9);

/* Sunset theme */
background: linear-gradient(135deg, #ff7f50, #ff6347);
```

### Challenge 2: Add Social Links
Add social media icons to your card:
```html
<div class="social-links">
    <a href="#" class="social-link">📧</a>
    <a href="#" class="social-link">📱</a>
    <a href="#" class="social-link">🎮</a>
</div>
```

### Challenge 3: Animation Effects
Add a pulse animation to your profile image:
```css
.profile-image {
    animation: pulse 2s infinite;
}

@keyframes pulse {
    0% { transform: scale(1); }
    50% { transform: scale(1.05); }
    100% { transform: scale(1); }
}
```

## 🔍 What Did You Learn?
- HTML document structure and semantic tags
- CSS box model and spacing
- Color gradients and shadows
- Hover effects and transitions
- Flexbox for centering content

## 🚀 Next Steps
Ready for more? Try Tutorial 2: Favorite Movies Gallery to learn about layouts and hover effects!