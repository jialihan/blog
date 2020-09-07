## Tips: DOM and Browser APIs
### 1. dataset: data- attribute
1 ) define the attribute in element
`data-whatever`:  
eg:
```
data-extra-info = "hello world"
```
2 ) read the data from the element
```
element.dataset; // DOMStringMap
element.dataset.extraInfo; // "hello world"
```

3 ) live-sync if we update dataset on js code
```
// add/set a new data- attribute on element
element.dataset.nameInfo = "jelly";
```
### 2. get element box dimension size
1 ) usage
```
element.getBoundingClientRect();
```
Result:
```
DOMRect {x: 831.5, y: 164, width: 272, height: 92, top: 164, …}
  bottom:  256
  height:  92
  left:  831.5
  right:  1103.5
  top:  164
  width:  272
  x:  831.5
  y:  164
  __proto__:  DOMRect
``` 

2 ) more position properties: **read-only**
outer positioning:
* element.**offsetLeft**
* element.**offsetTop**
inner positioning:
* element.**clientLeft**
* element.**clientTop**
entire size of this box including border and scrollbar
* element.**offsetWidth**
* element.**offsetHeight**
without border and scrollbar:
* element.**clientWidth**
* element.**clientHeight**
content size: scrollHeight gives you is the **entire height of the content** including the part which is currently not visible in the box.
* element.**scrollWidth**
* element.**scrollHeight**
scrollTop gives you information by how much you scroll that content in the box.
* element.**scrollTop**
window size:
* **window.innerWidth**: without the devtools and scroll bar
* **window.innerHeight**
* document.**documentElement.clientWidth**
*  document.**documentElement.clientHeight**
### 3. Dom Prototytpe
EventTarget <- Node <- Element <- HTMLElement <- HTMLInputElement

### 4. position the Tooltip
    tooltipElement.style.position = 'absolute';
    
    // get the parent element value
    const x = parentElement.offsetLeft + 20;
    const y = parentElement.offsetTop + height - 10;
    // set the new position value
    tooltipElement.style.left = x + 'px';
    tooltipElement.style.top = y + 'px';
    
### 5. handling scrolling

* element.**scrollTo**(x, y); // right, down: (0, 50): down 50px
* element.**scrollBy**(x, y);
* element.**scrollIntoView()**; // [Element/scrollIntoView](https://developer.mozilla.org/en-US/docs/Web/API/Element/scrollIntoView)

Add some behavior as tiny animation, ~~not in safari and IE~~, eg:
```
element.scrollIntoView({behavior: "smooth"});
element.scrollTo({ top: 50, behavior: "smooth"});
element.scrollBy({ top: 50, behavior: "smooth"});
```

### 6. `'<template>'` tag 

* the template tag is that its content by default is not rendered  
*  but it's part of the DOM, so you can query it and use it and then use it in your Javascript code but it's not getting rendered by default at the beginning.

1 ) how to import and refer it's content
use `importNode(content, true)`: if deep import
```
// do a deep importNode
const templateBody = document.importNode(tmpElement.content, true);
targetElement.append(templateBody);
```

### 7. load scripts dynamically
1 ) create a script element by code
```
const scriptElement = document.createElement('script');
scriptElement.textContent = 'alert("hi there!")';
document.head.append(scriptElement);
```

2 ) when you want to run your script at certain points
```
const analyticScript = document.createElment('script');
analyticScript.src = '..../analytic.js';
analyticScript.defer = true;
document.head.append(analyticScript);
```

### 8. Set timer and intervals

1 ) **setTimeout**
window.setTimeout() or just setTimeout( function, delay);
2 ) **setInterval**
* start : setInterval(function, time-interval);

3 ) how to **stop the timer**
* assign it to a variable
	```
	const timer = setTimout(...);
	const interval = setInterval(...);
	```
* when you want to stop it
	```
	clearTimeout(timer);
	clearInterval(interval);
	```

### 9. location & history object
* location
1 ) can be used to navigate: `location.href = "url"`
2 ) **location.replace**('url'); // but cannot go back
3 ) **location.host**
4 ) **location.origin**: [Web/API/Location/origin](https://developer.mozilla.org/en-US/docs/Web/API/Location/origin)
5 ) **location.pathname**; // without the origin, eg: "/", "/orders"


*  history
1 ) **history.back()**; // move user back to previous page
2 ) **history.forward()**; 
3 ) **history.length**:  how many steps the user did in this tab of the browser.
4 ) **history.go**(5):  go back how many steps

### 10. the navigator object
1 ) **navigator.userAgent**
2 ) **navigator.clipboard**
-  read()
-  readText()
 - write()
-  writeText()

3 ) get user's geolocation:
```
navigator.geolocation.getCurrentPosition(
	(data)=>{console.log(data);
})
```
4 ) decide which browser user is using:
Reference: [navigator#Example_1_Browser_detect_and_return_a_string](https://developer.mozilla.org/en-US/docs/Web/API/Window/navigator#Example_1_Browser_detect_and_return_a_string)

### 11. Date object
1 ) new Date(); // get current date and time
2) const date = new Date();
- date.getDay()
- date.getTime()
3 ) build a date: 
```
const date2 = new Date(date1);
const date3 = new Date('08/21/2020');
```

4 ) calculate difference of two days:
	 `milliseconds` =  date - date2;
	 
More on Date: [Global_Objects/Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date)
	 

