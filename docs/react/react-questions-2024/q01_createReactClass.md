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
