## Snake Game in JavaScript

#### I. [ Canvas Setup](#p1)  

#### II. [Game Setup](#p2)  

#### III. [Arrow Key's keyCode ](#p3)

#### IV. [Paint and Regenerate the Fruit](#p4) 

#### V. [Result & Source Code](#p5)

<div id="p1" />

### 1. Canvas Setup

HTML set up canvas element:
```html
<canvas id="gc" width="400" height="400"></canvas>
```
**JS global variables** to calculate about size in canvas:
Here we use a `20x20 = 400` grid, 20 cells in one row, each cell is a square box of  area `20*20 cell`.
```js
var x=0,y=0; // up,down,left,right by one cell
var gs = 20; // grid size
var tc = 20; // total count in one row
```

**How to paint with canvas?**
Docs: [fillStyle()](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/fillStyle), [fillRect()](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/fillRect)
For example: paint the background to be black color
```js
const canvas = document.getElementById('gc');
const ctx = canvas.getContext('2d');
ctx.fillStyle="black";
ctx.fillRect(0,0, canvas.width, canvas.height);
```

<div id="p2" />

### 2. Game Setup

- Player position: `var px=0, py=10;`
- Fruit position: `var fruitx=15,fruity=15;`
- body array with each cell position: `var body=[{x:10,y:10},....]`
- initial/original tail count: set to 5 `var tail = 5`
- Rule1: when never current position collide with body, it resets to start at current position, and tail becomes t o`5` as original.
	```js
	if(body[i].x === x && body[i].y === y)
	{
		// collision
		tail = 5;
	}
	```
- How to paint snake body ? 
	- `setInterval()`, everytime  render and paint the current all positions in `body[]` array to paint the latest position.
	- add new position, `while(body.Lenght>tail)`, pop out old positions.
	```js
	body.push({x:px, y:py});
	while(body.length>tail) {
		body.shift();
	}
	```

<div id="p3" />

### 3. Arrow Key's keyCode 

Arrow keys are only triggered by  `onkeydown`, not  `onkeypress`.

The keycodes are:

-   left = 37
-   up = 38
-   right = 39
-   down = 40

JS code to add event listener:
```js
document.addEventListener("keydown", function(e){
	const key = event.key; 
	switch (key) { 
		case "ArrowLeft": 
			x -= 1;
	        y = 0;
	        break; 
	    case "ArrowRight": 
            x +=1;
            y = 0;
            break; 
        case "ArrowUp": 
            y -= 1;
            x = 0;
            break; 
         case "ArrowDown": 
            y += 1;
            x = 0;
            break; 
	} 
});
```

<div id="p4" />

### 4. Paint and Regenerate the Fruit
1 ) check and expand fruit position to the snake body
Use [`Math.random()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/random): range 0 to less than 1 (inclusive of 0, but not 1)
```js
if(px === fruitx && py === fruity)
{
	// expand body
	tail++;
	fruitx = Math.floor(Math.random()*tc);
	fruity = Math.floor(Math.random()*tc);
}
```

2 ) render fruit position on canvas
use `gs-2` means a little bit gap of the cell (`gs * gs`), then we can have some margin on each cell.

```js
// fruit position
ctx.fillStyle="red";
ctx.fillRect(fruitx*gs,fruity*gs,gs-2,gs-2);
```

<div id="p5" />

### 5. Result & Source Code

![image](../assets/snakegame.png ':size=393x390')

**Source code:** [codepen-snake-game-JS](https://codepen.io/jellyhan27/pen/abBeZEa)