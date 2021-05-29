## Debounce and Throttle in JavaScript

#### I. [Debounce](#p1)  

#### II. [Throttle](#p2)  

#### III. [source code](#p3)

<div id="p1" />

### I. Debounce

#### 1.1 What is debounce?
Debouncing techniques is used to limit the number of times a function can execute. 
Debouncing makes that a function can’t be invoked again until a specific set of time has given without it being called.

Example: Execute a function only if 1000 milliseconds have passed without it being called.

Use cases:
-   Debouncing a  `resize`  event handler
-   Debouncing a  `scroll`  event handler
-   Debouncing a save function in an autosave feature 

#### 1.2 debounce function 

**Code:**
```js
function  debounce(fn, time) {
var  timer;
return  function (...args) {
clearTimeout(timer);
timer = setTimeout(() => {
fn(...args);
}, time);
}
}
```
**Usage example:**
```js
inputEL.addEventListener(
	'input', 
	debounce(onInputChangeHandler, 500)
);
```
#### 1.3 Debounce on input Demo

![image](../assets/debounce-cover.gif)

<div id="p2" />

### II. Throttle

#### 2.1 What is throttle?
Throttling techniques is used to limit the number of times a function can execute. 

Throttling will change the function in such a way that it can be fired at most once in a time interval.

For instance, throttling will execute the function only one time in 1000 milliseconds, no matter how many times the user clicks the button.

**Use cases:**
-   Throttling a  `scroll`  event handler
-   Throttling a button click so we can’t spam click
-   Throttling an API call
-   Throttling a  `mousemove`/`touchmove`  event handler

#### 2.2 throttle function 
**Code 1: simple throttle**
```js
function  throttle(fn, time) {
	var  isThrottle;
	return  function () {
		if (!isThrottle) {
			fn();
			isThrottle = true;
			setTimeout(() => {
				isThrottle = false;
			}, time);
		}	
	}
}
```

**Code 2: include leading & trailing edge triggering**
```js
// trigger on leading & trailing edge time

// last call on timelimit also run

function  throttle2(fn, limit) {
	var  lastCall;
	var  timer;
	return  function () {
		if (!lastCall) {
			// first invoke
			fn();
			lastCall = new  Date();
		}
		else {
			// in throttle
			clearTimeout(timer);
			timer = setTimeout(() => {
				if (Date.now() - lastCall >= limit) {
					fn();
					lastCall = new  Date();
				}
			}, limit - (Date.now() - lastCall));
		} 
	}
}
```
**Usage example:**
```js
containerEL.addEventListener(
	'scroll',
	throttle2(onScrollHandler, 500);
);
```

#### 2.3 Debounce on input Demo

![image](../assets/throttle-cover.gif)

<div id="p3" />

### III. Source Code

[github link](https://github.com/jialihan/JavaScript-Onboarding/tree/master/debounce_throttle)