# **DGL104 Process Portfolio**
## Ryan Dragon - N0180886
---
## **Activity 0101:**
> During the first semester of my Web and Mobile Application Development Diploma program, I struggled to work out what loops to use in specific situations. After learning and experiencing multiple coding languages, I learned the difference between loops such as for, for-in, while, etc. Once I started to become more comfortable with the code, I was able to confidently browse W3Schools and past assignments if I ever had issues come up again. With this deeper understanding I was able to effectively find solutions to a wide array of issues I have encountered over the last year and a half.
## **Activity 0102:**
### *Example Code:*

<details>
<summary>Click to Expand Code</summary>

```javascript
"use strict";

// Canvas references
const canvas = document.querySelector("canvas");
const ctx = canvas.getContext("2d");

// UI references
const restartButton = document.querySelector("#restart");
const undoButton = document.querySelector("#undo");
const colorSelectButtons = document.querySelectorAll(".color-select");

// Constants
const CELL_COLORS = {
    red: [255, 0, 0],
    blue: [0, 0, 255],
    white: [255, 255, 255],
};
const CELLS_PER_AXIS = 3;
const CELL_WIDTH = canvas.width / CELLS_PER_AXIS;
const CELL_HEIGHT = canvas.height / CELLS_PER_AXIS;

// Game objects
let replacementColor = CELL_COLORS.red;
let grids;

// Game Logic

function startGame(startingGrid = []) {
    if (startingGrid.length === 0) {
        startingGrid = initializeGrid();
    }
    initializeHistory(startingGrid);
    updateBoard();
    undoButton.disabled = true;
}

function initializeGrid() {
    const newGrid = [];
    for (let i = 0; i < CELLS_PER_AXIS * CELLS_PER_AXIS; i++) {
        newGrid.push(CELL_COLORS.white);
    }
    return newGrid;
}

function initializeHistory(startingGrid) {
    grids = [];
    grids.push(startingGrid);
}

function rollBackHistory() {
    if (grids.length > 0) {
        grids = grids.slice(0, grids.length - 1);
    }
    if (grids.length == 1) {
        undoButton.disabled = true;
    }
}

function renderCells(grid) {
    for (let i = 0; i < grid.length; i++) {
        ctx.fillStyle = `rgb(${grid[i][0]}, ${grid[i][1]}, ${grid[i][2]})`;
        ctx.fillRect(
            (i % CELLS_PER_AXIS) * CELL_WIDTH,
            Math.floor(i / CELLS_PER_AXIS) * CELL_HEIGHT,
            CELL_WIDTH,
            CELL_HEIGHT
        );
    }
}

function updateGridAt(mousePositionX, mousePositionY) {
    const gridCoordinates = convertCartesiansToGrid(
        mousePositionX,
        mousePositionY
    );
    const newGrid = grids[grids.length - 1].slice();
    squareFill(
        newGrid,
        gridCoordinates,
        newGrid[gridCoordinates.row * CELLS_PER_AXIS + gridCoordinates.column]
    );
    grids.push(newGrid);
    undoButton.disabled = false;
}

function squareFill(grid, gridCoordinate, colorToChange) {
    if (arraysAreEqual(colorToChange, replacementColor)) {
        return;
    } else {
        grid[
            gridCoordinate.row * CELLS_PER_AXIS + gridCoordinate.column
        ] = replacementColor;
    }
    return;
}

function renderLines() {
    ctx.beginPath();
    ctx.moveTo(200, 0);
    ctx.lineTo(200, 600);
    ctx.lineWidth = 5;
    ctx.stroke();
    ctx.beginPath();
    ctx.moveTo(400, 0);
    ctx.lineTo(400, 600);
    ctx.lineWidth = 5;
    ctx.stroke();
    ctx.beginPath();
    ctx.moveTo(0, 200);
    ctx.lineTo(600, 200);
    ctx.lineWidth = 5;
    ctx.stroke();
    ctx.beginPath();
    ctx.moveTo(0, 400);
    ctx.lineTo(600, 400);
    ctx.lineWidth = 5;
    ctx.stroke();
}

function restart() {
    startGame(grids[0]);
}

function updateBoard() {
    renderCells(grids[grids.length - 1]);
    renderLines();
}

// Event Listeners

canvas.addEventListener("mousedown", gridClickHandler);
function gridClickHandler(event) {
    updateGridAt(event.offsetX, event.offsetY);
    updateBoard();
}

restartButton.addEventListener("mousedown", restartClickHandler);
function restartClickHandler() {
    restart();
}

undoButton.addEventListener("mousedown", undoLastMove);
function undoLastMove() {
    rollBackHistory();
    updateBoard();
}

colorSelectButtons.forEach((button) => {
    button.addEventListener(
        "mousedown",
        () => (replacementColor = CELL_COLORS[button.name])
    );
});

// Helper Functions

// To convert canvas coordinates to grid coordinates
function convertCartesiansToGrid(xPos, yPos) {
    return {
        column: Math.floor(xPos / CELL_WIDTH),
        row: Math.floor(yPos / CELL_HEIGHT),
    };
}

// To compare two arrays
function arraysAreEqual(arr1, arr2) {
    if (arr1.length != arr2.length) {
        return false;
    } else {
        for (let i = 0; i < arr1.length; i++) {
            if (arr1[i] != arr2[i]) {
                return false;
            }
        }
        return true;
    }
}

//Start game
startGame();
```
</details>


### **3 Areas of Improvement:**
1. Comments aren't completely sufficient throughout the code. While there were comments, it would have been much more beneficial to explain each section of the code to further explain how it all works together.</br></br>
2. Loops located in rollBackHistory should be reformatted to a single if-esle loop, rather than two seperate if statements, this would be a much cleaner control flow.</br></br>
3. Indentation used to clean up some of the Functions/Arrays have irregular indentation. Many CONST elements and nested loops were not indented correctly, and correct indentation is absolutely essential when writing code.
---