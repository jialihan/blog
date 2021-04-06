## Event Listener

#### I. [WebAPI - Event Interface](#question1)

#### II. [addEventListener()](#question2)

#### III. [Input Event](#question3)

#### IV. [ Keyboard Event](#question4)

#### V. [Focus Event](#question5)

#### VI. [Improving scrolling performance with passive listeners](#question6)

#### VII. [Event Delegation](#question7)

<div id="question1" />

### I. WebAPI Event Interface

The [**Event**](https://developer.mozilla.org/en-US/docs/Web/API/Event) interface represents an event which takes place in the DOM.

**Important Properties:**
- [`Event.currentTarget`](https://developer.mozilla.org/en-US/docs/Web/API/Event/currentTarget)  Read only: is the element that the event listener is attached to. === `this`
- [`Event.target`](https://developer.mozilla.org/en-US/docs/Web/API/Event/target)  Read only:  is the original element that triggered the event (e.g., the user clicked on)

**Important Methods:**
- [`Event.preventDefault()`](https://developer.mozilla.org/en-US/docs/Web/API/Event/preventDefault): Cancels the event (if it is cancelable).

<div id="question2" />

### II. addEventListener()
Important and Common Parameters:

- `type`: A case-sensitive string representing the  [event type](https://developer.mozilla.org/en-US/docs/Web/Events)  to listen for.

- `listener`: a callback function receiving Event interface object
-  `options`  Optional
	The available options are:
	 - `capture` : `true or false`
	- `passive`
		A  [`Boolean`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean) that, if  `true`, indicates that the function specified by  `listener`  will never call  [`preventDefault()`](https://developer.mozilla.org/en-US/docs/Web/API/Event/preventDefault "preventDefault()"). If a passive listener does call  `preventDefault()`, the user agent will do nothing other than generate a console warning. See  [Improving scrolling performance with passive listeners](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener#improving_scrolling_performance_with_passive_listeners)  to learn more.
- useCapture Optional - true or false, default is `false`

**Usage example:**
```js
 someElement.addEventListener("click", function(e){},
 { passive: true}, false);
```

<div id="question3" />

### III. Input Event

If you're looking for a way to react to changes in an input's value, you should use the [`input`  event](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/input_event). Some changes are not detectable by `keyup`, for example pasting text from the context menu in a text input.

```js
inputEL.addEventlistener('input', function(e){});
```

<div id="question4" />

### IV. Keyboard Event

#### 4.1 Event
- [keyup](https://developer.mozilla.org/en-US/docs/Web/API/Element/keyup_event):
- [keydown](https://developer.mozilla.org/en-US/docs/Web/API/Document/keydown_event): The **`keydown`** event is fired when a key is pressed. eg: for **Arrow keys**, only triggered by `keydown` event
- ~~keypress~~: **~~deprecated, NOT USE~~**

#### 4.2 important properties
- [`KeyboardEvent.altKey`](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/altKey)  Read only: true/false
- [`KeyboardEvent.code`](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/code)  Read only: Returns a  [`DOMString`](https://developer.mozilla.org/en-US/docs/Web/API/DOMString)  with the code value of the physical key
- [`KeyboardEvent.ctrlKey`](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/ctrlKey)  Read only: true/false
- [`KeyboardEvent.key`](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/key)  Read only: Returns a  [`DOMString`](https://developer.mozilla.org/en-US/docs/Web/API/DOMString)  representing the key value of the key 
- [`KeyboardEvent.shiftKey`](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/shiftKey)  Read only: true/false

#### 4.3 Deprecated properties

- `KeyboardEvent.char`  Read only
- [`KeyboardEvent.charCode`](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/charCode)  Read only
- [`KeyboardEvent.keyCode`](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/keyCode)  Read only

<div id="question5" />

### 5. [Focus event](https://developer.mozilla.org/en-US/docs/Web/API/Element/focus_event)
The  **`focus`**  event fires when an element has received focus. The main difference between this event and  [`focusin`](https://developer.mozilla.org/en-US/docs/Web/API/Element/focusin_event "focusin")  is that  `focusin`  bubbles while  `focus`  does not.

The opposite of  `focus`  is  [`blur`](https://developer.mozilla.org/en-US/docs/Web/API/Element/blur_event "blur").

<div id="question6" />

### VI. Improving scrolling performance with passive listeners
**Docs**: [stackoverflow](https://stackoverflow.com/questions/37721782/what-are-passive-event-listeners), [mdn-event-listener](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener#improving_scrolling_performance_with_passive_listeners)

- problem: current browser need to wait for the results of any `touchstart` and `touchmove` handlers, which may prevent the scroll entirely by calling `preventDefault()` on the event.

- Solution: `{passive: true}`, by marking a touch or wheel listener as passive, the developer is promising the handler won't call `preventDefault` to disable scrolling.

<div id="question7" />

### VII. [Event Delegation](https://developer.mozilla.org/en-US/docs/Web/API/Element/focus_event#event_delegation)

#### 7.1 use bubbling phase
For example: [`focusin`](https://developer.mozilla.org/en-US/docs/Web/API/Element/focusin_event) can support bubbling on parent elements
```js
const form = document.getElementById('form');

form.addEventListener('focusin', (event) => {
  event.target.style.background = 'pink';
});
```

#### 7.2 use capture phase
For example: `focus` cannot bubble, but can use capture phase on parent element.
```js
form.addEventListener('focus', (event) => {
  event.target.style.background = 'pink';
}, true);

form.addEventListener('blur', (event) => {
  event.target.style.background = '';
}, true);
```

#### 7.3 Common use cases
DO NOT attach event listener on all sub elements like:
```html
<ul>
	<li></li>
	<li></li>
	<li></li>
</ul>
```
instead of each `liEL.addEventListener()`, just attach event listener on **parent element**:
```js
ulEL.addEventListener();
```
