## React Day11 - Important Concepts in React

#### I. [Before the React.js](#question-1)
  
#### II. [Declarative vs Imperative](#question-2)

#### III. [Component Architecture](#question-3)

#### VI. [One Way (Unidirectional) Data Flow](#question-4)
  
#### V. [UI Library](#question-5)

#### VI.[React Dev Skills](#question-6)

#### VII.[React - Client Side Rendering vs. SSR](#question-7)

#### VIII.[React - Controlled vs. Uncontrolled Components](#question-8)

<div  id="question-1"  />

### I. Before the React.js

|  tech stack | pros & cons |
|--|--|
| JS + HTML + CSS |  different browsers work differently|
| JQuery |  unified API to easily interact with DOM across all browsers |
|Backbone.js| |
|SPA - single page application| https://developer.mozilla.org/en-US/docs/Glossary/SPA |
| Angular JS | MVC or MVVM |


<div  id="question-2"  />

### II. Declarative vs Imperative
  
#### 2.1 JavaScript: manipulate the DOM
Some DOM manipulations:
- document.getElementById(ID)
- document.getElementByTagName(name)
- document.createElement(name)
- parentNode.appendChild(node)
- element.innerHTML
- element.style.left
- element.setAttribute()
- element.getAttribute()
- element.addEventListener()
- window.content
- window.onload
- window.dump()
- [window.scrollTo()](https://developer.mozilla.org/en-US/docs/Web/API/Window/scrollTo)

#### 2.2 Angular JS - imperative 

Imperative: how to do things, need to define how the DOM manipulation by developers. eg: add event listeners.

#### 2.3 react - declarative UI

**Declarative:** tell me what is your layout look like, React will take care of it to use/update/change the DOM by itself. dev won't change the DOM directly.

<div  id="question-3"  />

### III. Component Architecture

Design concept: **reusable components**
for example: 
- NavComponent
- ProfileComponent
- NestedComponent
-  ...

#### 3.1 What is component ?
component is just JavaScript a function or a class.

#### 3.2 react-dev-tools
[chrom extension](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en)

<div  id="question-4"  />

 
### IV. One Way (Unidirectional) Data Flow

Data only move in one direction, reference the following graph: 

**State+Component -> virtual DOM -> actual DOM** 

![image](../assets/unidirectionalFlow.png ':size=711.6x463')

<div id="question-5" />

### V. UI Library

we can use the **React idea & principles and same JS code** on cross platforms, desktop applications, VR applications, and mobile applications.

Important two libraries we import in React:
- ["react" library](https://www.npmjs.com/package/react)
- ["react-dom" library](https://www.npmjs.com/package/react-dom)

<div id="question-6" />

### VI. React Dev Skills

How to be a good react developer?
Things need to consider:
- decide on **Components**
	how to break down a component, how to build the re-usable component.
- decide the **state and where it lives**
	where the place that state lives in our app?
-  what changes when **state changes**
	consider the changes and re-renders, which related to performance

<div id="question-7" />

### VII. React - Client Side Rendering vs. SSR
-   SSR (traditional way) - server side rendering: server renders the page and returns fully compiled HTML
	- everytime we enter the new URL, client side receives whole new files of JS, HTML & CSS from server

		![image](../assets/ssr.png ':size=552x311')
    
-   [SPA](https://en.wikipedia.org/wiki/Single-page_application#:~:text=From%20Wikipedia,%20the%20free%20encyclopedia,browser%20loading%20entire%20new%20pages.) (React) - single page application: the page loads once, HTML is send to the client and JavaScript kicks in for all future interaction and update of the UI.
	- everytime **re-render** the page, only fetch new data, client side JS will be responsible to update the DOM and rendering.

		![image](../assets/singlepageapplication.png ':size=546x385')

-   CSR (React) - client side rendering: server returns (almost) empty HTML and JavaScript renders the page on the client's side

![image](../assets/rendering-on-the-web-seo-version.png)
	
<div id="question-8" />

#### VIII. Controlled vs. Uncontrolled Components

Docs: [stackoverflow-controlled-uncontrolled-components](https://stackoverflow.com/questions/42522515/what-are-react-controlled-components-and-uncontrolled-components)

- A [Controlled Component](https://reactjs.org/docs/forms.html#controlled-components):
	is one that takes its current value through props and notifies changes through callbacks like onChange. A parent component "controls" it by handling the callback and managing its own state and passing the new values as props to the controlled component. You could also call this a "dumb component".

- A [Uncontrolled Component](https://reactjs.org/docs/uncontrolled-components.html):
	is one that stores its own state internally, and you query 		the DOM using a ref to find its current value when you need it. This is a bit more like traditional HTML.


#### 8.1 controlled component 

An input form element whose value is controlled by React in this way is called a “controlled component”.
For example:
```jsx
<input type="text" value={value} onChange={handleChange} />
```

In its parent component:
In React, mutable state is typically kept in the state property of components, and only updated with [`setState()`](https://reactjs.org/docs/react-component.html#setstate).
```js
const [value, setValue] = useState('');
const handleChange = (event)=>{
	setValue(event.target.value);
}
```


#### 8.2 uncontrolled component

To write an uncontrolled component, instead of writing an event handler for every state update, you can [use a ref](https://reactjs.org/docs/refs-and-the-dom.html) to get form values from the DOM.

For example:
```jsx
const inputRef = React.createRef();
<input type="text" ref={inputRef} />
```

To get the current value, like the traditional DOM manipulation:
```js
const handleSubmit = (event) => {   
    event.preventDefault();
    console.log(inputRef.current.value); 
}
```