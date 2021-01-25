## React Day11 - Important Concepts in React

#### I. [Before the React.js](#question-1)
  
#### II. [Declarative vs Imperative](#question-2)

#### III. [Component Architecture](#question-3)

#### VI. [One Way (Unidirectional) Data Flow](#question-4)
  
#### V. [UI Library](#question-5)

#### VI.[React Dev Skills](#question-6)

#### VII.[Other Animation Packages (optional)](#question-7)

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
