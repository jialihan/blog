## React Day10 - Animations in React

#### I. [Use CSS Transitions](#question-1)
- [Syntax](#q1-1)
- [CSS code example](#q1-2)
- [Source Code](#q1-3)
  
#### II. [Use CSS Animations](#question-2)
- [Define the keyframes](#q2-1)
- [Use "animation" CSS property](#q2-2)
- [Source Code](#q2-3)

#### III. [Limitations using CSS transiton/animation in React](#question-3)

  
#### IV. [React-Transition-Group: use Transition Componnet](#question-4)
- [Install](#q4-1)
- [Transition Component](#q4-2)
- [use Transition Component on Modal Example](#q4-3)
- [Optimize: wrapping Transition Component inside Modal](#q4-4)
- [Animation Timing](#q4-5)
- [Transition Events - callback](#q4-6)
- [Source Code](#q4-7)
  
#### V. [React-Transition-Group: use CSSTransition Component](#question-5)
- [Code Syntax](#q5-1)
- [Customize the classNames](#q5-2)
- [Source Code](#q5-3)

#### VI.[React-Transition-Group: use TransitionGroup Component](#question-6)
- [Code Syntax](#q6-1)
- [Wrap CSSTransition Component on each Item](#q6-2)
- [Define  CSSTransition styles for each Item](#q6-3)
- [Source Code](#q6-4)

#### VII.[Other Animation Packages (optional)](#question-7)

<div  id="question-1"  />

### I. Use CSS Transitions

**Goal:**
add animations when modal open and closed, use `translate()` css property to make the modal fly in and fly out.

**Docs:**
https://www.w3schools.com/css/css3_transitions.asp

<div  id="q1-1"  />

#### 1.1 Syntax
```css
transition: transition-property | transition-duration | transition-timing-function | transition-delay;
```

<div  id="q1-2"  />

#### 1.2 CSS code example
Important Code: **"Modal.css"**
**1 ) Before the animation:**
```css
.Modal {
	/*...*/
}
.ModalOpen {
	display: block;
}
.ModalClosed {
	display: none;
}
```
**Note:**  **"display"** property cannot be animated in css !!!

**2 ) After the animation:**
```css
.Modal {
	/*...*/
	transition: all  1s  ease-out;
}

.ModalOpen {
	opacity: 1;
	transform: translateY(0);
}
.ModalClosed {
	opacity: 0;
	transform: translateY(-100%);
}
```

<div  id="q1-3"  /> 

#### 1.3 Source Code

[github link - Modal CSS transition](https://github.com/jialihan/React-features/blob/main/06-react-animations/css-transition-animation/src/components/Modal/Modal.css)

<div  id="question-2"  />

### II. Use CSS Animations
  
Goal: use `@keyframes` in css to define the specific behaviors for modal open and modal close, and use `animation` property in CSS.

**Docs:**
[@keyframes](https://developer.mozilla.org/en-US/docs/Web/CSS/@keyframes), ["animation" property](https://developer.mozilla.org/en-US/docs/Web/CSS/animation)

<div  id="q2-1"  />

#### 2.1  Define the keyframes
```css
@keyframes  openModal {
	0% {
		opacity: 0;
		transform: translateY(-100%);
	}
	50% {
		opacity: 1;
		transform: translateY(90%);
	}
	100% {
		opacity: 1;
		transform: translateY(0);
	}
}
@keyframes  closeModal {
	0% {
		opacity: 1;
		transform: translateY(0);
	}
	50% {
		opacity: 0.8;
		transform: translateY(60%);
	}
	100% {
		opacity: 0;
		transform: translateY(-100%);
	}
}
```

<div  id="q2-2"  />

#### 2.2 Use "animation" CSS property

**Syntax:**
```css
animation: @keyframes  name | duration | easing-function | delay;
```

**CSS Code Example:**
```css
.ModalOpen {
	animation: openModal 0.8s  ease-out  forwards;
}
.ModalClosed {
	animation: closeModal 0.8s  ease-out  forwards;
}
```

<div  id="q2-3"  />

#### 2.3 Source Code

[github link -Modal CSS animation](https://github.com/jialihan/React-features/blob/main/06-react-animations/css-transition-animation/src/components/Modal2/Modal.css)

<div  id="question-3"  />

### III. Limitations using CSS transition/animation in React

**Problem:**

we are using "opacity" to animate, but all these elements have to be in DOM, because "display: none" cannot be animated ! Problem comes when we un-mounting the component in react, and we want to really animate the "mount" & "unmount" components.

**Wrong Solution Code:**
```jsx
return (
	<div>
		{  isOpen ? <Modal  show={isOpen}  closed={closeModalHandler}  /> : null}
		{  isOpen ? <BackDrop  show={isOpen}  /> : null}
		<Button  variant="contained"  color="primary"  onClick={modalOpenHandler}>Open error modal</Button>
	</div>
)
```

  

**Reason:**

- mount the component: still works, because when doing the animation, the component is already rendered

- BUT: when remove the component, the **unmount re-render didn't wait until the animation finished.**

  

**Solution in React:**

See [React Transition Group](https://reactcommunity.org/react-transition-group/) and [React Motion](https://github.com/chenglou/react-motion) or [React Spring](https://github.com/react-spring/react-spring), for example.



<div  id="question-4"  />

 
### IV. Use React Transition Group

Docs: [react-transition-group](https://reactcommunity.org/react-transition-group/)

Components:
-   [Transition](https://reactcommunity.org/react-transition-group/transition)
-   [CSSTransition](https://reactcommunity.org/react-transition-group/css-transition)
-   [SwitchTransition](https://reactcommunity.org/react-transition-group/switch-transition)
-   [TransitionGroup](https://reactcommunity.org/react-transition-group/transition-group)

<div id="q4-1" />

#### 4.1 install 
```
# npm
npm install react-transition-group --save
```

<div id="q4-2" />

#### 4.2 Transition Component

it can control the animation of the elements inside of this components. 

**1 ) Syntax:**
```jsx
// import
import { Transition } from 'react-transition-group';
// usage
const  transitionStyles = {
	entering: { opacity:  0.3, transform:  'translateY(100%)' },
	entered: { opacity:  1, transform:  'translateY(0%)' },
	exiting: { opacity:  0.3 },
	exited: { opacity:  0 },
};

const element = (<Transition in={inProp} timeout={duration}>
    {state => (
      <div style={{
        ...transitionStyles[state]
      }}>
        <p> animation status: {state} </p>
      </div>
    )}
  </Transition>
);
```

**2 ) important control properties**
- `in`: transition components should be visible or not, only when in is "**true**", the components will be rendered.
- `timeout`: the duration between "ENTERING & ENTERED" and "EXITING & EXITED", how long it takes for enter & exit.
- `state`: it has internal props state to manage the 4 stages of the animation
	There are 4 main states a Transition can be in:
	-   `'entering'`
	-   `'entered'`
	-   `'exiting'`
	-   `'exited'`

	In code, for example, we can make use of the values of the state to control the component's **opacity**:
	```jsx
	<div style={
		opacity: state === 'entered' ? 1 : 0
	}></div>
	```
- [mountOnEnter](https://reactcommunity.org/react-transition-group/transition#Transition-prop-mountOnEnter):  the element will be **in the DOM** only when **entering/entered state**
- [unmountOnExit](https://reactcommunity.org/react-transition-group/transition#Transition-prop-unmountOnExit): unmount the element when state is **exited**, element won't in the DOM anymore.

**3 ) add animation using css transition attribute**
add transition css props:
for example: on all properties
```jsx
const  animationStyle = {
	transition:  `all 1000ms ease-out`
}
// inside Transition Component
{state => (
      <div style={{
	    ...animationStyle,
        ...transitionStyles[state]
      }}>
        <p> animation status: {state} </p>
      </div>
)}
```

<div id="q4-3" />

#### 4.3 use Transition Component on Modal Example

Use the internal "state" values to control the Modal css classes for open & close.
```jsx
// in component.jsx
<Transition  in={isOpen}  timeout={800}  mountOnEnter  unmountOnExit>
	{state  =>
	(<Modal  show={state}  closed={closeModalHandler}  />)}
</Transition>
```
Changes in Original Modal component to control the css styles use the `state` props:
```js
// modal.js
const  modal = props  => {
	const  cssClasses = [
	"Modal",
	props.show === 'entering' ? "ModalOpen" : props.show === 'exiting' ? "ModalClosed" : null
	];
	/* ... */
}
```

<div id="q4-4" />

#### 4.4 Optimize: wrapping Transition Component inside Modal

We can nested the `<Transition />` component inside any element, to make the code cleaner, we can wrap the `<Transition />` component inside the Modal. Result is the same.

```jsx
const  transitionModal = props  => {
return (
	<Transition  in={props.show}  timeout={800}  mountOnEnter  unmountOnExit>
	{  state  => {
		const  cssClasses = [
		"Modal",
		state === 'entering' ? "ModalOpen" : state === 'exiting' ? "ModalClosed" : null
		];
		return (
			<div  className={cssClasses.join(' ')}>
			<h1>A Modal with React Transiton Group</h1>
			<button  className="Button"  onClick={props.closed}>
			Dismiss
			</button>
			</div>
		)
	}}
	</Transition>
);
};
```

<div id="q4-5" />

#### 4.5 Animation Timing - timeout

The duration of the transition, in milliseconds. we can define the `timeout` property to be a JS object.
- single value: a single timeout for all transitions
- types: `appear`, `enter`, `exit`
	-   `appear`:  defaults to the value of  `enter`
	-   `enter`:  defaults to  `0`
	-   `exit`:  defaults to  `0`

<div id="q4-6" />

#### 4.6 Transition Events - callback

- [`onEnter`](https://reactcommunity.org/react-transition-group/transition#Transition-prop-onEnter)
- [`onEntering`](https://reactcommunity.org/react-transition-group/transition#Transition-prop-onEntering)
- [`onEntered`](https://reactcommunity.org/react-transition-group/transition#Transition-prop-onEntered)
- [`onExit`](https://reactcommunity.org/react-transition-group/transition#Transition-prop-onExit)
- [`onExiting`](https://reactcommunity.org/react-transition-group/transition#Transition-prop-onExiting)
- [`onExited`](https://reactcommunity.org/react-transition-group/transition#Transition-prop-onExited)

Code example:
```jsx
<Transition  in={isShow}
	timeout={duration}
	mountOnEnter
	unmountOnExit
	onEnter={() =>  console.log("on Eneter")}
	onEntering={() =>  console.log("on Enetering")}
	onEntered={() =>  console.log("on Enetered")}
	onExit={() =>  console.log("on Exit")}
	onExiting={() =>  console.log("on Exiting")}
	onExited={() =>  console.log("on Exited")} >
</Transition>
```

<div id="q4-7" />

#### 4.7 Source Code
[simple transition component example](https://github.com/jialihan/React-features/tree/main/06-react-animations/react-transition-group-example/src/components/SimpleTransitionComponentExample)
[modal with transition component](https://github.com/jialihan/React-features/blob/main/06-react-animations/react-transition-group-example/src/components/Modal/ModalWithTransition.jsx)

<div id="question-5" />

### V. React-Transition-Group: use CSSTransition Component

**Docs:** [css-transition](https://reactcommunity.org/react-transition-group/css-transition)

Have to use the `classNames` property to define:
- because we define which css classes should be added to the wrapped element, so to this `<div>` depending on the **state of the transition**.
- It will always **keep the original classes style** we on the element like modal in our case but then **merge these new
classes with that.**

<div id="q5-1" />

#### 5.1 Code Syntax
In component, use `<CSSTransition />` to wrap the elements, and add `classNames` property.
```jsx
<CSSTransition
	in={props.show}
	timeout={animationTiming}
	mountOnEnter
	unmountOnExit
	classNames="fade-modal"
>
	<div  className="Modal">
		<h1>A Modal with React Transiton Group</h1>
		<button  className="Button"  onClick={props.closed}>
		Dismiss
		</button>
	</div>
</CSSTransition>
```
In CSS file, need to define the each state on the `classNames="fade-modal"`.
```css
/* in css file */
.fade-modal-enter {
}
.fade-modal-enter-active {
	animation: openModal 0.4s  ease-out  forwards;
}
.fade-modal-exit {
}
.fade-modal-exit-active {
	animation: closeModal 1s  ease-out  forwards;
}
```

<div id="q5-2" />

#### 5.2 Customize the classNames
You can assign any other existing css classes to each state, using a JS object, then you don't need to define each state in CSS.
For example:
```js
classNames={
	{
		enter:  '',
		enterActive:  'ModalOpen',
		exit:  '',
		exitActive:  'ModalClosed'
	}
}
```

<div id="q5-3" />

#### 5.3 Source Code
[modal with CSSTransition Component](https://github.com/jialihan/React-features/blob/main/06-react-animations/react-transition-group-example/src/components/Modal/ModelWithCSSTransition.jsx)

<div id="question-6" />

### VI. React-Transition-Group: use TransitionGroup Component

Docs: [transition-group](https://reactcommunity.org/react-transition-group/transition-group)

When we render a group of elements, eg: unordered list, we can use `<TransitionGroup />` component to wrap them. The default component is `<div>`, here we need to change it to `<ul>`.

<div id="q6-1" />

####  6.1 Code Syntax
**Syntax:**
```jsx
<TransitionGroup>
{listItems}
</TransitionGroup>
```
**Important property:**
- [`component`](https://reactcommunity.org/react-transition-group/transition-group#TransitionGroup-prop-component)
	- type:  `any`
	- default:  `'div'`

<div id="q6-2" />

#### 6.2 Wrap CSSTransition Component on each Item

use `<CSSTransiton />` to wrap each **List Item**.

```jsx
<CSSTransition  key={index}  classNames="item"  timeout={500}>
	<li>...</li>
</CSSTransition>
```

**Note**: **Difference** is that we don't need to specify `in` property in  `<CSSTranstion />`, because here is a dynamic List, it will automatically manage the `in` property when we add / delete the item.

<div id="q6-3" />

#### 6.3 Define the CSS transition style for each **Item** 
```css
.item-enter {
	opacity: 0;
}
.item-enter-active {
	opacity: 1;
	transition: opacity 800ms  ease-in;
}
.item-exit {
	opacity: 1;
}
.item-exit-active {
	opacity: 0;
	transition: opacity 800ms  ease-out;
}
```

<div id="q6-4" />

#### 6.4 Source Code

[List with TransitionGroup Component](https://github.com/jialihan/React-features/blob/main/06-react-animations/react-transition-group-example/src/components/List/TransitionGroupList.jsx)

<div id="question-7" />

### VII. Other Animation Packages (optional)

-   Alternative => React Motion: [https://github.com/chenglou/react-motion](https://github.com/chenglou/react-motion)
-   Alternative => React Move: [https://github.com/react-tools/react-move](https://github.com/react-tools/react-move)
-   Animating Route Animations: [https://github.com/maisano/react-router-transition](https://github.com/maisano/react-router-transition)

