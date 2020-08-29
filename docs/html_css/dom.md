## DOM Manipulation

### I. Query nodes & elements 
* **Nodes**:  everything in the DOM is a node, eg: **textNode, elementNode, attributeNode**
*  **Elements**:  html tags are **Element Node**, not the text in there, have special methods to interact with the element, can be created and removed.

1. special property on document object that you can directly get.
	`document.body`  => Selects the  `<body>`  element node.
	`document.head`  => Selects the  `<head>` element node.
	`document.documentElement`  => Selects the  `<html>`  element node
	
2. Query Methods
* document.**querySelector(<CSS selector>)**
	Takes any **CSS selector** (e.g.  `'#some-id'`,  `'.some-class'`  or  `'div p.some-class'`) and returns the **first (!) matching element** in the DOM.  Returns  **null**  if no matching element could be found. 
* document.**getElementById(<ID>)**
Takes an ID (without  `#`, just the id name) and returns the element that has this id.  Returns  `null`  if no element with the specified ID could be found.
* document.**querySelectorAll(<CSS selector>)**
returns **all matching elements** in the DOM as a static (non-live) **NodeList**. Returns and empty  **NodeList**  if no matching element could be found.
* document.**getElementsByClassName(<CSS CLASS>)**
Takes a CSS class g (e.g.  `'some-class'`) and returns a live  **HTMLCollection**  of matched elements in your DOM. Returns an empty  **HTMLCollection** if not matching elements were found. More information: [Document/getElementsByClassName](https://developer.mozilla.org/en-US/docs/Web/API/Document/getElementsByClassName)
* document.**getElementsByTagName(<HTML TAG>)**
Takes an HTML tag (e.g.  `'p'`) and returns a live  **HTMLCollection** of matched elements in your DOM. Returns an empty  **HTMLCollection**  if not matching elements were found.

### II. Changing the DOM
1. get **textContent**: element.textContent = "text";
    behind the scenes: remove old textNode, create a new textNode, and replace the old one.
3. get **className**: element.className
4. get **style** peroperties:
   * element.**color** = 'white'
   * '-' character to camel case: element.**backgroundColor** = 'white';
   * ....more properties to explore: console.dir(element.style)
### III. Attributes vs. Properties
* Attribute: is added in html element tag, in your html text, html code.
* Property: is a value stored in the object that's created base on html code, automatically added on created DOM Objects.
 
 Mapping relationship:
 * 1-to-1 mapping and live-sync: eg: `id`
 * different names and live-sync: eg: `'class' attribute` maps to `'className' property`
  * 1-to-1 mapping but **one way live-sync**: 
  example in detail: `input.value`:
    - when user type something, reflect that the **property**: input.value, **changed**
    - but the original html code the **attribute**  `<input value="">` **not changed**
* force to change the value of the attribute: 
use method **setAttribute('attr_name', 'your new value')**

### IV. select multiple elements

1. querySelectorAll() -> return NodeList
	how to loop:
	- use `for( const el of nodeList )`
     - use index: `nodeList[0]`
    
    but this won't reflect the **live-sync** when we add/remove/changeContents of the elements, because **querySelector** is just the **snapshot** of the html document.
 
 2. getElementsByTagName() -> return a **live** [`HTMLCollection`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLCollection) of elements
  because an `HTMLCollection` in the HTML DOM is live; it is **automatically updated when the underlying document is changed**.
  
### V. Traverse the DOM
* Child: the direct child node or element
* Descendant: Direct or **indirect** child node or element
* Parent: the **direct parent** node or element
* Ancestor: direct or indirect parent node / element

Available api to access different nodes:
* select parent/ancestor:
	- parentNode: node
	- parentElement: only element can have a element child node, because we **cannot wrap textNode inside of a parent textNode**
	comparison of this two types on parent: pretty the same, but one exception
		```
		// exception case
		document.documentElement.parentElement//  = null
		document.documentElement.parentNode// = document object
		// normal case
		element.parentElement === element.parentNode
		```
	
	- closest(): return the **closest ancestor**: [API/Element/closest](https://developer.mozilla.org/en-US/docs/Web/API/Element/closest)
* select child nodes:
	 - childNodes: returns a **live** [`NodeList`](https://developer.mozilla.org/en-US/docs/Web/API/NodeList) of child [`nodes`](https://developer.mozilla.org/en-US/docs/Web/API/Node)
	 - children: returns a live [`NodeList`](https://developer.mozilla.org/en-US/docs/Web/API/NodeList) of child [`nodes`](https://developer.mozilla.org/en-US/docs/Web/API/Node)
	 comparison of this two objects:

	 ![image](../assets/nodelist.png ':size=499x386')
	 - querySelector()
	 - firstChild: return an node
	 - firstElementChild: return an element node
	 - lastChild: node
	 - lastElementChild: element
* sibling
	- previousSibling: node
	- previousElementSibling: element
	- nextSibling: node
	- nextElementSibling: element

### VI. Change the style of DOM Elements
* by **style** property
	```
	el.style.backgroundColor = 'green';
	```
* by **className**
	```
	el.className = "your-class-name";
	```
* by **classList**
	```
	el.classList.add('')
	el.classList.remove('')
	el.classList.replace('old-class', 'new-class')
	el.classList.toggle('')
	```
### VII. Create and Add Elements in DOM
1. Use HTML string:  `innerHTML, insertAdjacentHTML()`
2. document.**createElement('tagName')**: 
	```
	const newLi = document.createElement('li');
	newLi.textContent = 'new item';
	ulElement.appendChild(newLi);
	```
3.  add/insert element by methods:
	- appendChild():  accepts only Node objects
	- append(): accepts **Node objects** and **DOMStrings**, but **IE not support**
		```
		element.append('some text'); // correct 
		```
	- parentNode.prepend(): [API/ParentNode/prepend](https://developer.mozilla.org/en-US/docs/Web/API/ParentNode/prepend)
	- childNode.before():  [ChildNode/before](https://developer.mozilla.org/en-US/docs/Web/API/ChildNode/before), **not support on IE and safari**
	- childNode.after(): [ChildNode/after](https://developer.mozilla.org/en-US/docs/Web/API/ChildNode/after), **not support on IE and safari**
	- parentNode.**insertBefore(newNode, referenceNode)**:  [Node/insertBefore](https://developer.mozilla.org/en-US/docs/Web/API/Node/insertBefore)
	- parentNode.**replaceChild(_newChild_, _oldChild_)**: [Node/replaceChild](https://developer.mozilla.org/en-US/docs/Web/API/Node/replaceChild)
	- childNode.**replaceWith()**:  [ChildNode/replaceWith](https://developer.mozilla.org/en-US/docs/Web/API/ChildNode/replaceWith)
4. .textContent:  return any nested text
5. .innerHTML: replace everything inside this node, it will re-render, downside is: **user input might lose after the re-render the whole innerHTML**
6. insertAdjacentHTML('position', html_string):  [insertAdjacentHTML](https://developer.mozilla.org/en-US/docs/Web/API/Element/insertAdjacentHTML)
	-   `'beforebegin'`: Before the  `element`  itself
	- `'afterbegin'`: Just inside the  `element`, before its first child
	-  `'beforeend'`: Just inside the  `element`, after its last child
	-   `'afterend'`: After the  `element`  itself

### VIII. clone DOM elements
 element.**cloneNode(false/true)**:  deep clone or not as argument
### IX. remove DOM elements
```
el.parentElement.removeChild(el);
```

### X. static nodeList vs. live nodeList
* NodeList = querySelectorAll() : **static**: it take a **snapshot of DOM**
* HTMLCollection = el.getElementsByTagName(): **live**: includes most recent edition on elements
  eg: all of those api start with: document.**getElementsBySomething**, but **downside** is **memory consumption** if you manage a lot of collection.





  
	

