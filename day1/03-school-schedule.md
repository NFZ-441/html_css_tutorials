# Tutorial 3: School Schedule Table

## 🎯 Objective
Create an interactive, styled school timetable that displays your weekly schedule with color-coded subjects and hover effects.

## 📚 What You'll Learn
- HTML table structure and semantics
- Table styling and borders
- Color coding and visual hierarchy
- CSS pseudo-selectors
- Data organization and accessibility

## 🛠️ Step-by-Step Instructions

### Step 1: Create the HTML Structure
Create `index.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My School Schedule</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="container">
        <header class="schedule-header">
            <h1>📚 Year 10 School Schedule</h1>
            <p class="semester-info">Semester 2 • 2024</p>
        </header>

        <div class="schedule-wrapper">
            <table class="schedule-table">
                <thead>
                    <tr>
                        <th class="time-header">Time</th>
                        <th>Monday</th>
                        <th>Tuesday</th>
                        <th>Wednesday</th>
                        <th>Thursday</th>
                        <th>Friday</th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td class="time-slot">9:00 - 10:00</td>
                        <td class="subject math">
                            <div class="subject-info">
                                <span class="subject-name">Mathematics</span>
                                <span class="room-number">Room 201</span>
                                <span class="teacher">Ms. Johnson</span>
                            </div>
                        </td>
                        <td class="subject english">
                            <div class="subject-info">
                                <span class="subject-name">English</span>
                                <span class="room-number">Room 105</span>
                                <span class="teacher">Mr. Smith</span>
                            </div>
                        </td>
                        <td class="subject science">
                            <div class="subject-info">
                                <span class="subject-name">Physics</span>
                                <span class="room-number">Lab 3</span>
                                <span class="teacher">Dr. Wilson</span>
                            </div>
                        </td>
                        <td class="subject math">
                            <div class="subject-info">
                                <span class="subject-name">Mathematics</span>
                                <span class="room-number">Room 201</span>
                                <span class="teacher">Ms. Johnson</span>
                            </div>
                        </td>
                        <td class="subject history">
                            <div class="subject-info">
                                <span class="subject-name">History</span>
                                <span class="room-number">Room 308</span>
                                <span class="teacher">Mr. Brown</span>
                            </div>
                        </td>
                    </tr>

                    <tr>
                        <td class="time-slot">10:15 - 11:15</td>
                        <td class="subject english">
                            <div class="subject-info">
                                <span class="subject-name">English</span>
                                <span class="room-number">Room 105</span>
                                <span class="teacher">Mr. Smith</span>
                            </div>
                        </td>
                        <td class="subject science">
                            <div class="subject-info">
                                <span class="subject-name">Chemistry</span>
                                <span class="room-number">Lab 2</span>
                                <span class="teacher">Ms. Davis</span>
                            </div>
                        </td>
                        <td class="subject math">
                            <div class="subject-info">
                                <span class="subject-name">Mathematics</span>
                                <span class="room-number">Room 201</span>
                                <span class="teacher">Ms. Johnson</span>
                            </div>
                        </td>
                        <td class="subject tech">
                            <div class="subject-info">
                                <span class="subject-name">Digital Tech</span>
                                <span class="room-number">Computer Lab</span>
                                <span class="teacher">Mr. Garcia</span>
                            </div>
                        </td>
                        <td class="subject pe">
                            <div class="subject-info">
                                <span class="subject-name">PE</span>
                                <span class="room-number">Gymnasium</span>
                                <span class="teacher">Coach Lee</span>
                            </div>
                        </td>
                    </tr>

                    <tr class="break-row">
                        <td class="time-slot break">11:15 - 11:30</td>
                        <td colspan="5" class="break-time">☕ Morning Break</td>
                    </tr>

                    <tr>
                        <td class="time-slot">11:30 - 12:30</td>
                        <td class="subject science">
                            <div class="subject-info">
                                <span class="subject-name">Biology</span>
                                <span class="room-number">Lab 1</span>
                                <span class="teacher">Dr. Green</span>
                            </div>
                        </td>
                        <td class="subject math">
                            <div class="subject-info">
                                <span class="subject-name">Mathematics</span>
                                <span class="room-number">Room 201</span>
                                <span class="teacher">Ms. Johnson</span>
                            </div>
                        </td>
                        <td class="subject english">
                            <div class="subject-info">
                                <span class="subject-name">English</span>
                                <span class="room-number">Room 105</span>
                                <span class="teacher">Mr. Smith</span>
                            </div>
                        </td>
                        <td class="subject history">
                            <div class="subject-info">
                                <span class="subject-name">History</span>
                                <span class="room-number">Room 308</span>
                                <span class="teacher">Mr. Brown</span>
                            </div>
                        </td>
                        <td class="subject art">
                            <div class="subject-info">
                                <span class="subject-name">Art</span>
                                <span class="room-number">Art Studio</span>
                                <span class="teacher">Ms. Taylor</span>
                            </div>
                        </td>
                    </tr>

                    <tr class="break-row">
                        <td class="time-slot lunch">12:30 - 1:30</td>
                        <td colspan="5" class="lunch-time">🍕 Lunch Break</td>
                    </tr>

                    <tr>
                        <td class="time-slot">1:30 - 2:30</td>
                        <td class="subject tech">
                            <div class="subject-info">
                                <span class="subject-name">Digital Tech</span>
                                <span class="room-number">Computer Lab</span>
                                <span class="teacher">Mr. Garcia</span>
                            </div>
                        </td>
                        <td class="subject pe">
                            <div class="subject-info">
                                <span class="subject-name">PE</span>
                                <span class="room-number">Gymnasium</span>
                                <span class="teacher">Coach Lee</span>
                            </div>
                        </td>
                        <td class="subject art">
                            <div class="subject-info">
                                <span class="subject-name">Art</span>
                                <span class="room-number">Art Studio</span>
                                <span class="teacher">Ms. Taylor</span>
                            </div>
                        </td>
                        <td class="subject english">
                            <div class="subject-info">
                                <span class="subject-name">English</span>
                                <span class="room-number">Room 105</span>
                                <span class="teacher">Mr. Smith</span>
                            </div>
                        </td>
                        <td class="subject science">
                            <div class="subject-info">
                                <span class="subject-name">Chemistry</span>
                                <span class="room-number">Lab 2</span>
                                <span class="teacher">Ms. Davis</span>
                            </div>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>

        <div class="legend">
            <h3>Subject Legend</h3>
            <div class="legend-items">
                <div class="legend-item math">Mathematics</div>
                <div class="legend-item english">English</div>
                <div class="legend-item science">Science</div>
                <div class="legend-item history">History</div>
                <div class="legend-item tech">Digital Tech</div>
                <div class="legend-item pe">PE</div>
                <div class="legend-item art">Art</div>
            </div>
        </div>
    </div>
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
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    min-height: 100vh;
    padding: 20px;
}

.container {
    max-width: 1200px;
    margin: 0 auto;
    background: white;
    border-radius: 20px;
    box-shadow: 0 20px 40px rgba(0, 0, 0, 0.1);
    overflow: hidden;
}

/* Header */
.schedule-header {
    background: linear-gradient(135deg, #2c3e50, #3498db);
    color: white;
    text-align: center;
    padding: 30px 20px;
}

.schedule-header h1 {
    font-size: 2.5rem;
    margin-bottom: 10px;
    text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
}

.semester-info {
    opacity: 0.9;
    font-size: 1.1rem;
}

/* Table wrapper */
.schedule-wrapper {
    overflow-x: auto;
    padding: 30px;
}

/* Table styling */
.schedule-table {
    width: 100%;
    border-collapse: collapse;
    border-radius: 10px;
    overflow: hidden;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
}

/* Table header */
.schedule-table thead th {
    background: linear-gradient(135deg, #34495e, #2c3e50);
    color: white;
    padding: 15px;
    font-weight: 600;
    text-align: center;
    position: relative;
}

.schedule-table thead th:first-child {
    background: linear-gradient(135deg, #e74c3c, #c0392b);
}

/* Time column */
.time-slot {
    background: #f8f9fa !important;
    font-weight: bold;
    text-align: center;
    color: #2c3e50;
    width: 120px;
    padding: 15px 10px;
}

/* Subject cells */
.subject {
    padding: 15px;
    border: 1px solid #eee;
    transition: all 0.3s ease;
    cursor: pointer;
    position: relative;
}

.subject:hover {
    transform: scale(1.02);
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
    z-index: 10;
}

/* Subject info layout */
.subject-info {
    display: flex;
    flex-direction: column;
    gap: 4px;
}

.subject-name {
    font-weight: bold;
    font-size: 1rem;
}

.room-number {
    font-size: 0.8rem;
    opacity: 0.8;
}

.teacher {
    font-size: 0.75rem;
    font-style: italic;
    opacity: 0.7;
}

/* Subject color coding */
.subject.math {
    background: linear-gradient(135deg, #ff9a9e, #fecfef);
    color: #8b1538;
}

.subject.english {
    background: linear-gradient(135deg, #a8edea, #fed6e3);
    color: #0f4c75;
}

.subject.science {
    background: linear-gradient(135deg, #d299c2, #fef9d7);
    color: #6b3e9a;
}

.subject.history {
    background: linear-gradient(135deg, #89f7fe, #66a6ff);
    color: #1e3799;
}

.subject.tech {
    background: linear-gradient(135deg, #fa709a, #fee140);
    color: #8b2635;
}

.subject.pe {
    background: linear-gradient(135deg, #43e97b, #38f9d7);
    color: #0d5f3c;
}

.subject.art {
    background: linear-gradient(135deg, #fa8072, #ffb347);
    color: #8b3626;
}

/* Break and lunch styling */
.break-row td {
    text-align: center;
    font-weight: bold;
    padding: 10px;
}

.break-time {
    background: linear-gradient(135deg, #ffd89b, #19547b);
    color: white;
    font-size: 1.1rem;
}

.lunch-time {
    background: linear-gradient(135deg, #667eea, #764ba2);
    color: white;
    font-size: 1.1rem;
}

.time-slot.break,
.time-slot.lunch {
    background: #34495e !important;
    color: white !important;
}

/* Legend */
.legend {
    padding: 30px;
    background: #f8f9fa;
    border-top: 3px solid #3498db;
}

.legend h3 {
    color: #2c3e50;
    margin-bottom: 15px;
    text-align: center;
}

.legend-items {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 10px;
}

.legend-item {
    padding: 8px 15px;
    border-radius: 20px;
    font-size: 0.9rem;
    font-weight: 500;
}

/* Responsive design */
@media (max-width: 768px) {
    .schedule-header h1 {
        font-size: 1.8rem;
    }
    
    .schedule-wrapper {
        padding: 15px;
    }
    
    .schedule-table {
        font-size: 0.8rem;
    }
    
    .subject {
        padding: 10px 5px;
    }
    
    .legend-items {
        flex-direction: column;
        align-items: center;
    }
    
    .legend-item {
        width: 200px;
        text-align: center;
    }
}

/* Animation for loading */
@keyframes slideInDown {
    from {
        opacity: 0;
        transform: translateY(-30px);
    }
    to {
        opacity: 1;
        transform: translateY(0);
    }
}

.schedule-table {
    animation: slideInDown 0.6s ease;
}

/* Highlight current day */
.schedule-table tbody tr:hover {
    background-color: rgba(52, 152, 219, 0.1);
}
```

### Step 3: Customize Your Schedule
1. Replace the subjects with your actual classes
2. Update teacher names and room numbers
3. Adjust time slots to match your school schedule
4. Add or remove subjects as needed

## 💡 Tips & Tricks
- Use `colspan` to span cells across multiple columns
- Color coding helps organize information visually
- Hover effects make tables interactive
- `border-collapse: collapse` removes gaps between cells

## 🎨 Creative Challenges

### Challenge 1: Add Assignment Tracker
Add upcoming assignments to each subject:
```html
<div class="subject-info">
    <span class="subject-name">Mathematics</span>
    <span class="room-number">Room 201</span>
    <span class="assignment">📝 Quiz: Friday</span>
</div>
```

### Challenge 2: Current Time Indicator
Highlight the current time slot:
```css
.current-time {
    border: 3px solid #f39c12;
    box-shadow: 0 0 20px rgba(243, 156, 18, 0.5);
}
```

### Challenge 3: Subject Difficulty Rating
Add difficulty indicators with stars or colors:
```html
<span class="difficulty">⭐⭐⭐</span>
```

### Challenge 4: Export Button
Add a print-friendly version:
```css
@media print {
    body { background: white; }
    .schedule-table { box-shadow: none; }
}
```

## 🔍 What Did You Learn?
- HTML table structure and semantics
- CSS gradients and color schemes
- Responsive table design
- Hover effects and transitions
- Data organization principles
- CSS pseudo-classes

## 🚀 Next Steps
Ready for Tutorial 4? Create a Digital Recipe Book to learn about lists and typography!