### controlled component vs. uncontrolled component

In most cases, we recommend using [controlled components](https://legacy.reactjs.org/docs/uncontrolled-components.html) to implement forms.

1. key difference: `React` can or cannot know the `value/state changes` in those elements, eg: `input`, `select`.

- controlled component: form data is handled by a React component, use **two way binding**, writing an `event handler` for every state
- uncontrolled components: form data is handled by the DOM itself, you can use a `ref` to get form values from the DOM.

2. how to change a uncontrolled component to be controlled?

controlled element:

```jsx
const [age, setAge] = useState(18);
<input value={age} onChange={(e) => setAge(e.target.value)} />;
```

uncontrolled element:

```jsx
const inputRef = useRef(null);
<input defaultValue="16" type="text" ref={inputRef} />;
```

3. Props only for uncontrolled component

for `<input />` element ([doc](https://react.dev/reference/react-dom/components/input#props)), these are only for uncontrolled inputs:

- `defaultChecked`: A boolean. Specifies the initial value for `type="checkbox"` and `type="radio"` inputs.
- `defaultValue`: A string. Specifies the initial value for a text input.

For example, even if you assign a different `defaultValue` on a controlled component, it won't work:

```jsx
const [age, setAge] = useState(18);
<input
  defaultValue="16"
  type="text"
  value={age}
  onChange={(e) => setAge(e.target.value)}
/>;
// UI result showing: 18
```

But if you don't have a default value in `useState()`, then it will pick up the `defaultValue` prop:

```jsx
const [age, setAge] = useState();
<input
  defaultValue="16"
  type="text"
  value={age}
  onChange={(e) => setAge(e.target.value)}
/>;
// UI result showing: 16
```
