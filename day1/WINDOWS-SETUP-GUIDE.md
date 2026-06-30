# How to Run HTML & CSS Tutorials on Windows

## 🖥️ Method 1: Using File Explorer (Easiest)

### Step 1: Create Your Project Folder
1. Open **File Explorer** (Windows key + E)
2. Navigate to your Documents folder or Desktop
3. Right-click and select **New > Folder**
4. Name it something like "HTML-Tutorials"

### Step 2: Create HTML and CSS Files
1. Open **Notepad** (Search "Notepad" in Start menu)
2. Copy the HTML code from any tutorial
3. Paste it into Notepad
4. Save as `index.html` (make sure to change "Save as type" to "All Files")
5. Create another Notepad file for CSS
6. Save it as `style.css`

### Step 3: Open in Browser
1. Double-click the `index.html` file in File Explorer
2. It will open in your default browser (Edge, Chrome, Firefox)
3. Make changes to files and refresh the page to see updates

## 🔧 Method 2: Using Better Text Editors

### Option A: Visual Studio Code (Recommended)
1. **Download VS Code**: Go to https://code.visualstudio.com/
2. **Install**: Run the downloaded file and follow setup
3. **Open VS Code** and click "Open Folder"
4. **Select your project folder**
5. **Create new files**: Right-click in sidebar > New File
6. **Live Preview**: Install "Live Server" extension for auto-refresh

### Option B: Notepad++
1. **Download**: Go to https://notepad-plus-plus.org/
2. **Install** and open Notepad++
3. **Better syntax highlighting** than regular Notepad
4. **Create files** with proper extensions (.html, .css)

## 🌐 Method 3: Online Editors (No Installation)

### CodePen (Great for Beginners)
1. Go to https://codepen.io/
2. Click "Create" > "New Pen"
3. Add HTML in HTML panel
4. Add CSS in CSS panel
5. See results instantly!

### Other Online Options
- **JSFiddle**: https://jsfiddle.net/
- **CodeSandbox**: https://codesandbox.io/
- **Repl.it**: https://replit.com/

## 📁 Windows File Structure Example

```
📂 Documents/
  📂 HTML-Tutorials/
    📂 Tutorial-1-Profile-Card/
      📄 index.html
      📄 style.css
    📂 Tutorial-2-Movie-Gallery/
      📄 index.html
      📄 style.css
    📂 Tutorial-3-School-Schedule/
      📄 index.html
      📄 style.css
```

## 🎯 Quick Start Guide for Windows

### For Complete Beginners:
1. **Right-click on Desktop** > New > Folder > Name it "WebProjects"
2. **Open Notepad** (Windows + R, type "notepad", press Enter)
3. **Copy this starter code**:
```html
<!DOCTYPE html>
<html>
<head>
    <title>My First Page</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <h1>Hello World!</h1>
    <p>This is my first webpage!</p>
</body>
</html>
```
4. **Save as** "index.html" in your WebProjects folder
5. **Create another Notepad file** with:
```css
body {
    font-family: Arial;
    background-color: lightblue;
    text-align: center;
}
h1 {
    color: darkblue;
}
```
6. **Save as** "style.css" in the same folder
7. **Double-click index.html** to open in browser

## 🔧 Windows Keyboard Shortcuts

- **Ctrl + S**: Save file
- **F5**: Refresh browser page
- **Ctrl + Shift + I**: Open browser developer tools
- **Alt + Tab**: Switch between applications
- **Windows + E**: Open File Explorer

## 🌟 Pro Tips for Windows Users

### 1. Enable File Extensions
1. Open **File Explorer**
2. Click **View** tab
3. Check **"File name extensions"**
4. Now you can see .html and .css extensions

### 2. Set Default Program
1. Right-click any .html file
2. Select **"Open with" > "Choose another app"**
3. Select your preferred browser
4. Check **"Always use this app"**

### 3. Create Desktop Shortcuts
1. Right-click your project folder
2. Select **"Send to" > "Desktop (create shortcut)"**
3. Quick access to your projects!

### 4. Use Windows Snipping Tool
- **Windows + Shift + S**: Screenshot your webpage results
- Great for sharing your work with teachers/friends

## 🎨 Recommended Browser Setup

### Chrome DevTools (Best for Learning)
1. **Open your webpage** in Google Chrome
2. **Press F12** or right-click > "Inspect"
3. **Elements tab**: See your HTML structure
4. **Styles panel**: Edit CSS live and see changes instantly
5. **Console**: Check for errors

### Edge DevTools
- Similar to Chrome, built into Windows
- **F12** to open developer tools
- Good alternative if you prefer Edge

## 🔍 Troubleshooting Common Windows Issues

### Problem: HTML file opens as text
**Solution**: Right-click > Open with > Choose browser

### Problem: CSS not loading
**Solution**: Check that both files are in the same folder and CSS filename matches exactly

### Problem: Changes not showing
**Solution**: Press F5 to refresh the browser page

### Problem: Can't see file extensions
**Solution**: File Explorer > View > Check "File name extensions"

## 🚀 Next Steps

1. **Start with Tutorial 1** (Personal Profile Card)
2. **Create a folder** for each tutorial
3. **Save your work** regularly (Ctrl + S)
4. **Take screenshots** of your results
5. **Experiment** with the code - change colors, text, sizes!

## 💡 Windows-Specific File Paths

When working with images or links, use forward slashes:
```css
/* Good */
background-image: url("images/photo.jpg");

/* Also works on Windows */
background-image: url("./images/photo.jpg");
```

Happy coding on Windows! 🎉