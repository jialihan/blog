## React Day09 - Styles in React (CSS & Styled-Component)

#### I. [Ways to Style Component in React](#question-1)

#### II. [Inline Styles in React Component](#question-2)

#### III. [Normal CSS classes in React Component](#question-3)

#### IV. [Styled-Components: 3rd party library](#question-4)
- [Installation](#q4-1)
- [Import](#q4-2)
- [Create Styled Component](#q4-3)
- [useful tools in VSCode](#q4-4)
- [Optional: Animation](#q4-5)
#### V. [Reference: Source Code](#question-5)



<div id="question-1" />

### I. Ways to Style Component in React

- Inline CSS - not recommended
- Normal CSS - externa l CSS file & use `className` attribute
- CSS-in-JS:  3rd party libraries, eg: [styled-components](https://styled-components.com/docs/basics)

How to do animations? 
- For example:  [React Transition Group](https://reactcommunity.org/react-transition-group/) and [React Motion](https://github.com/chenglou/react-motion) or [React Spring](https://github.com/react-spring/react-spring)


<div id="question-2" />

### II. Inline Styles in React Component

Docs: [styles in react](https://reactjs.org/docs/dom-elements.html#style)

**Note:** this is **NOT** recommended ! (for performance and for clean code)

Code Example:
```js
export  default  function  InlineCSSComponent() {
	const  h1Style = {
		color:  'blue',
		border:  '1px solid blue'
	}
	return (
		<h1  style={h1Style}>
			I am inline-CSS style.
		</h1>
	);
}
```

<div id="question-3" />

### III. Normal CSS classes in React Component

Docs: [react-styling](https://reactjs.org/docs/faq-styling.html)

**Syntax:**
use attribute `className` 

**Import external CSS file:**
```
import './myComponent.css';
```

**Code  example:**
css file is the same as external files:
```css
/* external css file */
.MyHeadline {
	color: indianred;
	padding: 10px;
	margin: 20px;
	border: 1px  dashed  indianred;
}
```
```js
export  default  function  Normalcsscomponent(props) {
	return (
		<h1  className="MyHeadline">
			I am normal CSS class style
		</h1>
	)
}
```

<div id="question-4" />

### IV. Styled-Components: 3rd party library

 **Preface: [motivations](https://styled-components.com/docs/basics#motivation)** 
A way to mix CSS-in-Javascript, instead of separating them.
Motivation to use this pattern:
- automatic critical CSS: user only load the necessary (real-needed) css attached to the components.
- no class name bugs: **no duplication**, local css class names
- simple dynamic styling: with some **"if-else" logic** or checking logic, we can insert JS into css.
- **automatic vendor prefixing**: this package do it for you

<div id="q4-1" />

#### 4.1 installation
```
npm install --save styled-components
```

<div id="q4-2" />

#### 4.2 import 
```
import  styled  from  'styled-components';
```

<div id="q4-3" />

#### 4.3 Create Styled Component

1 ) A simple component: 
eg: `<h1>`, `<div>`, `<p>` 
```js
const  B = styled.div`
	height: 500px;
	width: 50%;
	margin: 10px auto;
	border: 1px dotted green;
`;
```

2 ) Add props logic on CSS
**pass a function** to style component's template to adapt it based on props.
```js
const  Button = styled.button`
	border: 2px solid green;
	font-size: 22px;
	color: white;
	padding: 5px 10px;
	margin-left: 30px;
	background: ${props  =>  props.isPrimary ? 'green' : 'white'};
`;
```

3 ) [Extending/Inherit Styles](https://styled-components.com/docs/basics#extending-styles)

Just wrap it in the `styled()` constructor, can you can extend any other styled component. For example, we might have different types of buttons:
- eg1: "DangerButton" extends "Button"
	```js
	const  DangerButton = styled(Button)`
		color: red;
		font-weight: bold;
	`;
	```
- eg2: "ReversedButton" extends "DangerButton"
	```jsx
	const  ReversedButton = props  =>  <DangerButton  
		{...props}  
		children={props.children.split('').reverse()}  
	/>; 
	```

4 ) Important: it's a good practice to "**Define Styled Components outside of the render method"**

For example: a styled wrapper component
```jsx
// define
const StyledWrapper = styled.div`
 /* TODO */
`;

// usage
const Wrapper = ({ message }) => {
  return <StyledWrapper>{message}</StyledWrapper>
}
```

5 ) [pseudo selectors](https://styled-components.com/docs/basics#pseudoelements-pseudoselectors-and-nesting) & nested

ampersand `&` char can be used to refer back to the main component. 
Example 1: pseudo selector
```css
const  Button = styled.button`
	/* ... */
	padding: 5px  10px;
	margin-left: 30px;
	&:hover,
	&:active {
		background: yellow;
	}
`;
```
Example 2: nested styles
```css
const Container = styled.button`
	/* ... */
	padding: 5px  10px;
	margin-left: 30px;
	&.SomeClass {
		background: orange;
	}
`;
```
usage of nested component is like the following code:
```jsx
// usage
const myButton = ( 
	<Container>
	<div className="SomeClass"></div>
	</Container>
);
```

<div id="q4-4" />

#### 4.4 useful tools in VSCode

extension in VSCode: [vscode-styled-components](https://marketplace.visualstudio.com/items?itemName=jpoissonnier.vscode-styled-components)

<div id="q4-5" />

#### 4.5 Optional: Animation 

**Docs:** [animation- styled-component](https://styled-components.com/docs/basics#animations)

1 ) Create the keyframes
Note: remember to import from this library:
```js
import  styled, { keyframes } from  'styled-components';

const rotate = keyframes`
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
`;
```

2 ) create the animated component
```js
const  RotateComponent = styled.div`
	display: inline-block;
	animation: ${rotate}  2s  linear  infinite;
	padding: 2rem  1rem;
	font-size: 2rem;
`;
// usage
return (
	<RotateComponent>Hello World</RotatedComponent>
);
```

<div id="question-5" />

### V. Reference: Source Code

React code example: [github link](https://github.com/jialihan/React-features/tree/main/05-styled-components/styles-in-react)






