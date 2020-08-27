## Understand the DOM and DOM Manipulation

### Query nodes & elements 
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

### Changing the DOM
1. get **textContent**: element.textContent = "text";
    behind the scenes: remove old textNode, create a new textNode, and replace the old one.
3. get **className**: element.className
4. get **style** peroperties:
   * element.**color** = 'white'
   * '-' character to camel case: element.**backgroundColor** = 'white';
   * ....more properties to explore: console.dir(element.style)
### Attributes vs. Properties
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

### select multiple elements

1. querySelectorAll() -> return NodeList
	how to loop:
	- use `for( const el of nodeList )`
     - use index: `nodeList[0]`
    
    but this won't reflect the **live-sync** when we add/remove/changeContents of the elements, because **querySelector** is just the **snapshot** of the html document.
 
 2. getElementsByTagName() -> return a **live** [`HTMLCollection`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLCollection) of elements
  because an `HTMLCollection` in the HTML DOM is live; it is **automatically updated when the underlying document is changed**.



  
	

