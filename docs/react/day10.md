## React Day10 - Animations in React 

#### I. [Use CSS Transitions](#question-1)

#### II. [Use CSS Animations](#question-2)

#### III. [Limitations using CSS transiton/animation in React](#question-3)

#### IV. [Use React Transition Group](#question-4)

<div id="question-1" />

### I. Use CSS Transitions

**Goal:**
 add animations when modal open and closed, use `translate()` css  property to make the modal fly in and fly out.

**Docs:**
 https://www.w3schools.com/css/css3_transitions.asp

**Syntax:**
```
transition: transition-property | transition-duration | transition-timing-function | transition-delay;
```

Important Code:  **"Modal.css"**
**1 ) Before the animation:**
```css
.Modal {
	...
}
.ModalOpen {
	display: block;
}
.ModalClosed {
	display: none;
}
```
**Note:** **"display"** property cannot be animated in css !!!

**2 ) After the animation:**
```css
.Modal {
	...
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

**Source Code:**

[github link - Modal CSS transition](https://github.com/jialihan/React-features/blob/main/06-react-animations/css-transition-animation/src/components/Modal/Modal.css)

<div id="question-2" />

### II. Use CSS Animations

Goal: use `@keyframes` in css to define the specific behaviors for  modal open and modal close, and use `animation` property in CSS.

**Docs:**
 [@keyframes](https://developer.mozilla.org/en-US/docs/Web/CSS/@keyframes),  ["animation" property](https://developer.mozilla.org/en-US/docs/Web/CSS/animation) 

#### Step1: Define the keyframes:
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

#### Step2: use "animation" css property
**Syntax:**
```css
animation: @keyframes name | duration | easing-function | delay;
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

**Source Code:**

[github link -Modal CSS animation](https://github.com/jialihan/React-features/blob/main/06-react-animations/css-transition-animation/src/components/Modal2/Modal.css)

<div id="question-3" />

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
See  [React Transition Group](https://reactcommunity.org/react-transition-group/)  and  [React Motion](https://github.com/chenglou/react-motion)  or  [React Spring](https://github.com/react-spring/react-spring), for example.

<div id="question-4" />

### IV. Use React Transition Group

Docs: [react-transition-group](https://reactcommunity.org/react-transition-group/)