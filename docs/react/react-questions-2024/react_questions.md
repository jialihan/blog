## 1. [createReactClass()](https://legacy.reactjs.org/docs/react-without-es6.html) vs React.Component

❌ createReactClass - before ES6, not encouraged, very less usage in `react` github, only some pure compoennt use it, eg: ReactArt-Rectangle.

#### 1.1 diff on syntax, functions in createReactClass

```js
var createReactClass = require("create-react-class");
var Greeting = createReactClass({
  getDefaultProps: function () {
    return {
      name: "Mary",
    };
  },

  getInitialState: function () {
    return { count: this.props.initialCount };
  },

  render: function () {
    return <h1>Hello, {this.props.name}</h1>;
  },
});
```

#### 1.2 Not need to bind any method in createRreactClass.

But in ES6 Component, 2 ways to bind the methods:

```js
// way 1
constructor() {
    this.handler = this.handler.bind(this);
}
// way 2: use arrow func
onClick={(e) => this.handleClick(e)}
```

#### 1.3 ES6 doesn't support [Mixins](https://legacy.reactjs.org/docs/react-without-es6.html#mixins)

ES6 launched without any mixin support. Therefore, there is no support for mixins when you use React with ES6 classes.

**React Community doesn’t recommend using them in the new code.**
just example code for mixIns:

```js
var SetIntervalMixin = {
  componentWillMount: function () {
    this.intervals = [];
  },
  setInterval: function () {
    this.intervals.push(setInterval.apply(null, arguments));
  },
  componentWillUnmount: function () {
    this.intervals.forEach(clearInterval);
  },
};

var createReactClass = require("create-react-class");

var TickTock = createReactClass({
  mixins: [SetIntervalMixin], // Use the mixin
  getInitialState: function () {
    return { seconds: 0 };
  },
  componentDidMount: function () {
    this.setInterval(this.tick, 1000); // Call a method on the mixin
  },
  tick: function () {
    this.setState({ seconds: this.state.seconds + 1 });
  },
  render: function () {
    return <p>React has been running for {this.state.seconds} seconds.</p>;
  },
});

const root = ReactDOM.createRoot(document.getElementById("example"));
root.render(<TickTock />);
```

Why mixins are Harmful?

- [mixins introduce implicit dependencis](https://legacy.reactjs.org/blog/2016/07/13/mixins-considered-harmful.html#mixins-introduce-implicit-dependencies)
- [mixins cause snowballing complexity](https://legacy.reactjs.org/blog/2016/07/13/mixins-considered-harmful.html#mixins-cause-snowballing-complexity): You can’t easily hoist the state used by mixin up into the parent component. Components using the same mixin become increasingly coupled with time.

延伸：VUE also not recommend Mixins:

> No Longer Recommended
> In Vue 2, mixins were the primary mechanism for creating reusable chunks of component logic. While mixins continue to be supported in Vue 3, Composable functions using [Composition API](https://vuejs.org/guide/reusability/composables.html) is now the preferred approach for code reuse between components.

#### 1.4 React.Component - ES6 classes

But nowadays, recommend Functional component.

> We recommend defining components as functions instead of classes. See [how to migrate](https://react.dev/reference/react/Component#alternatives).

```js
class Greeting extends Component {
  render() {
    return <h1>Hello, {this.props.name}!</h1>;
  }
}
```

### 2. React event object([doc](https://react.dev/reference/react-dom/components/common#react-event-object)) vs. DomEvent

[SyntheticEvent](https://github.com/facebook/react/blob/v18.2.0/packages/react-native-renderer/src/legacy-events/SyntheticEvent.js#L61):

2.1 naming convention

- `onClick={}`: camel cases
- native event: all small cases, eg: `onclick, onsubmit`

  2.2. passing the function

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

  2.3 Not a 1-to-1 mapping of DomEvent, eg: in `onMouseLeave`, `e.nativeEvent` will point to a `mouseout` event.  
   If you need the underlying browser event for some reason, read it from e.nativeEvent.

  2.4 React event objects implement some of the standard `Event`([mdn doc](https://developer.mozilla.org/en-US/docs/Web/API/Event)) properties: - bubbles: A boolean. Returns whether the event bubbles through the DOM. - cancelable: A boolean. Returns whether the event can be canceled. - `currentTarget`: A DOM node. Returns the node to which the current handler is attached in the React tree. - `defaultPrevented`: A boolean. Returns whether preventDefault was called. - eventPhase: A number. Returns which phase the event is currently in. - isTrusted: A boolean. Returns whether the event was initiated by user. - `target`: A DOM node. Returns the node on which the event has occurred (which could be a distant child). - timeStamp: A number. Returns the time when the event occurred.

  2.5 Benefis of SyntheticEvent(before v17) - [doc](https://blog.saeloun.com/2021/04/06/react-17-removes-event-pooling-in-modern-system/)

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
