## React design pattern

### 1. controlled vs uncontrolled component

Why prefer controlled component?

- easy to test, can mock states and pass to test
- more re-usable
- parent has no control of the un-controlled component
  ```
  // parent
  <>
    <Modal /> // uncontrolled
    <button />
  <>
  ```

### 4. HOC

- to share complexity behavior between components
- to add functionality on existing components

Note:

- small letter at the beginning, since we never load it directly, we call it as a function, eg: `const HOC = withUser(UserComponent, '123')`;
- Examples of use-case: edit server data, form's `onChange`
  - review: 2 ways of onChange function:
    - a) use Arrow function: `onChange={(e)=>this.onChange()}`
    - b) bind `this` manually in constructor : `this.onChange = this.onChange.bind(this);`

### 5. Functional Programming in React

- controlled components
- function components
- Higher-order components
- **Recursive components**
  - For example: load a nested object by recursion to render with `<li>, <ul>`, eg: `obj= { a: {a1: 1, a2: 2}, b:{...} }`.
- **Component composition**
  Create different versions of a component by creating new components that use the original component.
  for example:
  ```
  const Btn = ({ size, color, text, ...props}) => {
  	return (
  		<button style={{
  			padding: size==='large' ? 32px : 16px,
  			backgroundColor: color,
  		}} {...props}>
  			{text}
  		</button>
  	);
  }
  ```
  consume this basic components:
  ```
  export const DangerButton = props => {
  	return (<Btn {...props} color="red" />);
  }
  export const BigSuccessButton = props => {
  	return (<Btn {...props} size='large' color="green" />);
  }
  ```
- **Partially applied components**
  a way to allow the user to specify a set number of props on a component without creating a new component
  `export const partiallyApplied = (Component, partialProps) => {
	return props => {
		<Component {...partialProps} {...props} />
	}
}
// usage
export const DangerBtn = partiallyApplied(Btn, {color: 'red'});
export const BigSuccessBtn = partiallyApplied(Btn, {color:'green', size:'large'});`
