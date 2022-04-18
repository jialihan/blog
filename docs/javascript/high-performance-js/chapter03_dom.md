## Chapter 3 DOM Scripting

The three categories of problems discussed in the chapter include:

- Accessing and modifying DOM elements
- Modifying the styles of DOM elements and causing repaints and reflows
- Handling user interaction through DOM events

### 1. DOM Access & Modification

#### 1.1 innerHTML vs. DOM method

`innerHTML` is faster than other DOM methods, eg: `document.createElement()`.

For example: when you want to update 1000 rows of data

```text
// innerHTML
you will write <tr>/<td> tags
// methods
tr = document.createElement('tr');
...
```

#### 1.2 Cloning Nodes

`element.cloneNode()` is faster than `document.createElment()`.

#### 1.3 HTML Collections

Slow

```js
// loop every item directly
for (var count = 0; count < len; count++) {
  name = document.getElementsByTagName("div")[count].nodeName;
}
```

Faster

```js
// loop a copy of collection
const copy = document.getElementsByTagName("div");
for (var count = 0; count < len; count++) {
  name = copy[count].nodeName;
}
```

**Fastest**: use local variable when access collection elements

```js
const copy = document.getElementsByTagName("div");
for (var count = 0; count < len; count++) {
  var el = copy[count];
  name = el.nodeName;
}
```

#### 1.4 Walking the DOM

- When crawling the DOM, eg: `nextSibling` performs much better than `childNodes`.
- Use "Element nodes" than the whole nodes, which might contain other type of nodes eg: textNode.

  | Property               | Use as a replacement for |
  | ---------------------- | ------------------------ |
  | children               | childNodes               |
  | childElementCount      | childNodes.length        |
  | firstElementChild      | firstChild               |
  | lastElementChild       | lastChild                |
  | nextElementSibling     | nextSibling              |
  | previousElementSibling | previousSibling          |

- Selectors API (`querySelector(), querySelectorAll()`): which returns a static list of elements like an array object. They are faster than live HTML collection list elements.

### 2. Repaints and Reflows

- A DOM tree
  A representation of the page structure
- A render tree
  A representation of how the DOM nodes will be displayed

When a DOM change affects the geometry of an element (width and height)—such as a change in the thickness of the border or adding more text to a paragraph, resulting in an additional line, the "Reflow" happens.

When does a Reflow happen?

- Visible DOM elements are added or removed
- Elements change position
- Elements change size (because of a change in margin, padding, border thickness, width, height, etc.)
- Content is changed, e.g., text changes or an image is replaced with one of a different size
- Page renders initially-
- Browser window is resized

#### 2.1 Queuing and Flushing Render Tree Changes

Inefficient way of modifying the same property:

```js
bodystyle.color = "red";
tmp = computed.backgroundColor;
bodystyle.color = "white";
tmp = computed.backgroundImage;
bodystyle.color = "green";
tmp = computed.backgroundAttachment;
```

Faster way:

```js
bodystyle.color = "red";
bodystyle.color = "white";
bodystyle.color = "green";
tmp = computed.backgroundColor;
tmp = computed.backgroundImage;
tmp = computed.backgroundAttachment;
```

#### 2.2 Minimizing Repaints and Reflows

- **Style changes:**
  use css to accumulate the changes
  ```
  var el = document.getElementById('mydiv');
  el.style.borderLeft = '1px';
  el.style.borderRight = '2px';
  el.style.padding = '5px';
  ```
  Better way to do it:
  ```
  var el = document.getElementById('mydiv');
  el.style.cssText = 'border-left: 1px; border-right: 2px; padding: 5px;';
  ```
- Batching DOM changes
  Take the element out of the DOM, then modify it, finally bring it back to DOM.
  There are **three** basic ways to modify the DOM off the document:
  - **Hide** the element, apply changes, and show it again.
    ```
    ul.style.display = 'none';
    ```
  - Use a document **fragment** to build a subtree outside of the live DOM and then copy it to the document.
    ```
    fragment = document.createDocumentFragment();
    ```
  - Copy the original element into an off-document node, modify the copy, and then replace the original element once you’re done.
    ```
    var clone = old.cloneNode(true);
    ```

#### 2.3 Caching Layout Information

For example:

```
current++; // cached in this variable
myElement.style.left = current + 'px';
myElement.style.top = current + 'px';
if (current >= 500) {
    stopAnimation();
}
```

#### 2.4 Take Elements Out of the Flow for Animations

Use **absolute** positioning for the element you want to animate on the page, taking it out of the layout flow of the page.

### 3. Event Delegation

use Bubble or Capture phase to delegate the event handler on the parent elements, but IE do not support Capture, it's better to use Bubbling.

For example:
it's better to attach the `onclick` eventHandler on the `ul` parent element instead attaching 3 eventHandler on each `li` element.

```
<ul>
	<li></li>
	<li></li>
	<li></li>
</ul>
```

### 4. Summary

- Minimize DOM access, and try to work as much as possible in JavaScript.

- Use local variables to store DOM references you’ll access repeatedly.

- Be careful when dealing with HTML collections because they represent the live, underlying document. Cache the collection `length` into a variable and use it when iterating, and make a copy of the collection into an array for heavy work on collections.

- Use faster APIs when available, such as `querySelectorAll()` and `firstElementChild`.

- Be mindful of repaints and reflows; batch style changes, manipulate the DOM tree “offline,” and cache and minimize access to layout information.

- Position absolutely during animations.

- Use event delegation to minimize the number of event handlers.
