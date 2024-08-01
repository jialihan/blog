## 2. React event object([doc](https://react.dev/reference/react-dom/components/common#react-event-object)) vs. DomEvent

[SyntheticEvent](https://github.com/facebook/react/blob/v18.2.0/packages/react-native-renderer/src/legacy-events/SyntheticEvent.js#L61):

### 2.1 naming convention

- `onClick={}`: camel cases
- native event: all small cases, eg: `onclick, onsubmit`

### 2.2. passing the function

- `onClick={hanlder}` or arrow function `onClick={()=>handler()}`
- native event: a string of fucntion name:

```html
<button onclick="myFunction()">Click me</button>
<script>
  function myFunction() {
    document.getElementById("demo").innerHTML = "Hello World";
  }
</script>
```

### 2.3 Not a 1-to-1 mapping of DomEvent

eg: in `onMouseLeave`, `e.nativeEvent` will point to a `mouseout` event. If you need the underlying browser event for some reason, read it from e.nativeEvent.

### 2.4 React event objects implement some of the standard `Event`([mdn doc](https://developer.mozilla.org/en-US/docs/Web/API/Event)) properties

- bubbles: A boolean. Returns whether the event bubbles through the DOM.
- cancelable: A boolean. Returns whether the event can be canceled.
- `currentTarget`: A DOM node. Returns the node to which the current handler is attached in the React tree. - `defaultPrevented`: A boolean. Returns whether preventDefault was called.
- eventPhase: A number. Returns which phase the event is currently in.
- isTrusted: A boolean. Returns whether the event was initiated by user.
- `target`: A DOM node. Returns the node on which the event has occurred (which could be a distant child).
- timeStamp: A number. Returns the time when the event occurred.

### 2.5 Benefis of SyntheticEvent(before v17) - [doc](https://blog.saeloun.com/2021/04/06/react-17-removes-event-pooling-in-modern-system/)

**Before react 17:**

when an event is triggered, React takes an instance from the pool, populates its properties and, reuses it.

To assure consistent usage of the pooled events, **React nullifies the properties of synthetic events** ([PR](https://github.com/facebook/react/pull/18216/files)) right after executing an event handler.

**Note: !!! IMPORTANT CHANGE**

> Reason:
> Though Event Pooling was built to increase the performance it **didn’t improve the performance in modern browsers**. It also **confused developers**. For example, not being to access eventx.target in the setState updater.

In React 17, the same code works as expected allowing us to fetch event.target.value without calling event.persist().

The old event pooling optimization has been fully removed, so we can read the event fields whenever we need them.

```js
 handleChange(event) {
    console.log(event.target.value);
    this.setState(() => ({
      text: event.target.value // GOOD!!!
    }));
  }
```
