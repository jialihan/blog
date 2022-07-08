## React Basics Concept

### 1. What is [JSX](https://reactjs.org/docs/introducing-jsx.html)?

It is called JSX, and it is a syntax extension to JavaScript. JSX produces React “elements”.

#### 1.1 JSX Prevents Injection Attacks

It is safe to embed user input in JSX:

```
const title = response.potentiallyMaliciousInput;
// This is safe:
const element = <h1>{title}</h1>;
```

By default, React DOM [escapes](https://stackoverflow.com/questions/7381974/which-characters-need-to-be-escaped-on-html) any values embedded in JSX before rendering them. Thus it ensures that you can never inject anything that’s not explicitly written in your application. Everything is converted to a string before being rendered. This helps prevent [XSS (cross-site-scripting)](https://en.wikipedia.org/wiki/Cross-site_scripting) attacks.

#### 1.2 JSX Represents Objects

Babel compiles JSX down to `React.createElement()` calls.

These two examples are identical:

```
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
```

```
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```

`React.createElement()` performs a few checks to help you write bug-free code but essentially it creates an object like this:

### 2. What is Data-binding?

**Data Binding** is the process of connecting the view element or user interface, with the data which populates it.

**One-way Data Binding:** ReactJS uses one-way data binding. In one-way data binding one of the following conditions can be followed:

- **Component to View:** Any change in component data would get reflected in the view.
- **View to Component:** Any change in View would get reflected in the component’s data.

Manually implement 2-way data binding in react:

```jsx
function Component1() {
  let [inputValue, setInputValue] = useState("");
  let changeValue = (e) => setInputValue(e.target.value);
  return (
    <div>
      <input value={inputValue} onChange={changeValue} />
    </div>
  );
}
```
