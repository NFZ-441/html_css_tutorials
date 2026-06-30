# Tutorial 2: Favorite Movies Gallery

## 🎯 Objective
Build an interactive movie gallery that displays your favorite films with cool hover effects and responsive grid layout.

## 📚 What You'll Learn
- CSS Grid and Flexbox layouts
- Hover effects and transforms
- Image handling and overlay effects
- CSS transitions and animations
- Responsive design basics

## 🛠️ Step-by-Step Instructions

### Step 1: Create the HTML Structure
Create `index.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Favorite Movies</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <header class="page-header">
        <h1 class="main-title">🎬 My Favorite Movies</h1>
        <p class="subtitle">A collection of films that inspire me</p>
    </header>

    <main class="movies-container">
        <div class="movie-card">
            <div class="movie-poster">
                <img src="https://via.placeholder.com/300x450/ff6b6b/ffffff?text=Movie+1" alt="Movie 1">
                <div class="movie-overlay">
                    <h3 class="movie-title">The Matrix</h3>
                    <p class="movie-year">1999</p>
                    <p class="movie-genre">Sci-Fi • Action</p>
                    <div class="movie-rating">⭐⭐⭐⭐⭐</div>
                </div>
            </div>
        </div>

        <div class="movie-card">
            <div class="movie-poster">
                <img src="https://via.placeholder.com/300x450/4ecdc4/ffffff?text=Movie+2" alt="Movie 2">
                <div class="movie-overlay">
                    <h3 class="movie-title">Spider-Man</h3>
                    <p class="movie-year">2021</p>
                    <p class="movie-genre">Action • Adventure</p>
                    <div class="movie-rating">⭐⭐⭐⭐⭐</div>
                </div>
            </div>
        </div>

        <div class="movie-card">
            <div class="movie-poster">
                <img src="https://via.placeholder.com/300x450/45b7d1/ffffff?text=Movie+3" alt="Movie 3">
                <div class="movie-overlay">
                    <h3 class="movie-title">Avengers</h3>
                    <p class="movie-year">2019</p>
                    <p class="movie-genre">Action • Adventure</p>
                    <div class="movie-rating">⭐⭐⭐⭐⭐</div>
                </div>
            </div>
        </div>

        <div class="movie-card">
            <div class="movie-poster">
                <img src="https://via.placeholder.com/300x450/f7b731/ffffff?text=Movie+4" alt="Movie 4">
                <div class="movie-overlay">
                    <h3 class="movie-title">Inception</h3>
                    <p class="movie-year">2010</p>
                    <p class="movie-genre">Sci-Fi • Thriller</p>
                    <div class="movie-rating">⭐⭐⭐⭐⭐</div>
                </div>
            </div>
        </div>

        <div class="movie-card">
            <div class="movie-poster">
                <img src="https://via.placeholder.com/300x450/5f27cd/ffffff?text=Movie+5" alt="Movie 5">
                <div class="movie-overlay">
                    <h3 class="movie-title">Black Panther</h3>
                    <p class="movie-year">2018</p>
                    <p class="movie-genre">Action • Drama</p>
                    <div class="movie-rating">⭐⭐⭐⭐⭐</div>
                </div>
            </div>
        </div>

        <div class="movie-card">
            <div class="movie-poster">
                <img src="https://via.placeholder.com/300x450/00d2d3/ffffff?text=Movie+6" alt="Movie 6">
                <div class="movie-overlay">
                    <h3 class="movie-title">Guardians</h3>
                    <p class="movie-year">2014</p>
                    <p class="movie-genre">Action • Comedy</p>
                    <div class="movie-rating">⭐⭐⭐⭐⭐</div>
                </div>
            </div>
        </div>
    </main>

    <footer class="page-footer">
        <p>Created by [Your Name] | Year 10 Digital Technologies</p>
    </footer>
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
    font-family: 'Arial', sans-serif;
    background: linear-gradient(135deg, #1e3c72, #2a5298);
    color: white;
    line-height: 1.6;
    min-height: 100vh;
}

/* Header styles */
.page-header {
    text-align: center;
    padding: 40px 20px;
    background: rgba(0, 0, 0, 0.3);
    backdrop-filter: blur(10px);
}

.main-title {
    font-size: 3rem;
    margin-bottom: 10px;
    text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
}

.subtitle {
    font-size: 1.2rem;
    opacity: 0.8;
}

/* Movies container - CSS Grid */
.movies-container {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
    gap: 30px;
    padding: 40px 20px;
    max-width: 1400px;
    margin: 0 auto;
}

/* Individual movie card */
.movie-card {
    perspective: 1000px;
}

.movie-poster {
    position: relative;
    border-radius: 15px;
    overflow: hidden;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
    transition: all 0.3s ease;
    cursor: pointer;
}

.movie-poster:hover {
    transform: translateY(-10px) rotateX(5deg);
    box-shadow: 0 20px 50px rgba(0, 0, 0, 0.5);
}

.movie-poster img {
    width: 100%;
    height: 400px;
    object-fit: cover;
    transition: transform 0.3s ease;
}

.movie-poster:hover img {
    transform: scale(1.1);
}

/* Movie overlay */
.movie-overlay {
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    background: linear-gradient(transparent, rgba(0, 0, 0, 0.9));
    color: white;
    padding: 30px 20px 20px;
    transform: translateY(100%);
    transition: transform 0.3s ease;
}

.movie-poster:hover .movie-overlay {
    transform: translateY(0);
}

.movie-title {
    font-size: 1.5rem;
    margin-bottom: 5px;
    font-weight: bold;
}

.movie-year {
    color: #ffd700;
    font-weight: bold;
    margin-bottom: 5px;
}

.movie-genre {
    opacity: 0.8;
    margin-bottom: 10px;
    font-size: 0.9rem;
}

.movie-rating {
    font-size: 1.2rem;
}

/* Footer */
.page-footer {
    text-align: center;
    padding: 30px 20px;
    background: rgba(0, 0, 0, 0.3);
    margin-top: auto;
}

/* Responsive design */
@media (max-width: 768px) {
    .main-title {
        font-size: 2rem;
    }
    
    .movies-container {
        grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
        gap: 20px;
        padding: 20px;
    }
    
    .movie-poster {
        height: 350px;
    }
}

/* Loading animation */
@keyframes fadeInUp {
    from {
        opacity: 0;
        transform: translateY(30px);
    }
    to {
        opacity: 1;
        transform: translateY(0);
    }
}

.movie-card {
    animation: fadeInUp 0.6s ease forwards;
}

.movie-card:nth-child(1) { animation-delay: 0.1s; }
.movie-card:nth-child(2) { animation-delay: 0.2s; }
.movie-card:nth-child(3) { animation-delay: 0.3s; }
.movie-card:nth-child(4) { animation-delay: 0.4s; }
.movie-card:nth-child(5) { animation-delay: 0.5s; }
.movie-card:nth-child(6) { animation-delay: 0.6s; }
```

### Step 3: Customize Your Gallery
1. Replace placeholder movies with your actual favorites
2. Update movie titles, years, and genres
3. Change the star ratings to match your opinions
4. Add real movie poster URLs if you want

## 💡 Tips & Tricks
- `CSS Grid` automatically arranges items in responsive columns
- `transform: perspective()` creates 3D effects
- `backdrop-filter: blur()` creates modern glass effects
- Animation delays create a staggered loading effect

## 🎨 Creative Challenges

### Challenge 1: Add More Movie Info
Extend the overlay with additional details:
```html
<div class="movie-overlay">
    <h3 class="movie-title">Movie Title</h3>
    <p class="movie-director">Director: Christopher Nolan</p>
    <p class="movie-duration">Duration: 148 min</p>
    <p class="movie-description">A brief plot summary...</p>
</div>
```

### Challenge 2: Filter Buttons
Add genre filter buttons:
```html
<div class="filter-buttons">
    <button class="filter-btn active" data-filter="all">All</button>
    <button class="filter-btn" data-filter="action">Action</button>
    <button class="filter-btn" data-filter="sci-fi">Sci-Fi</button>
    <button class="filter-btn" data-filter="comedy">Comedy</button>
</div>
```

### Challenge 3: Card Flip Effect
Create a flip card that shows movie details on the back:
```css
.movie-card {
    transform-style: preserve-3d;
    transition: transform 0.6s;
}

.movie-card:hover {
    transform: rotateY(180deg);
}
```

### Challenge 4: Add a "Watch Later" Button
```html
<button class="watch-later-btn">📝 Add to Watchlist</button>
```

## 🔍 What Did You Learn?
- CSS Grid for responsive layouts
- Advanced hover effects and transforms
- Image overlay techniques
- CSS animations and transitions
- 3D transform effects
- Responsive design principles

## 🚀 Next Steps
Ready for Tutorial 3? Learn how to create organized data displays with a School Schedule Table!