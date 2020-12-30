## React day04 - Redux: Basics

I. [What is State?](#question-1)

II. [Understand Redux Data Flow](#question-2)

III. [Set up Redux in React project](#question-3)
 - [Import Redux to application](#q3-1)
 - [Set up Reducer and Store](#q3-2)
 - [Dispatch Actions](#q3-3)
 - [Add Subscriptions](#q3-4)

IV. [Connecting Store to the React App](#question-4)
- [Install react-redux package](#q4-1)
- [Import the Provider](#q4-2)
- [Connecting Store to the react Component](#q4-3)
- [mapStateToProps](#q4-4)
- [mapDispatchToProps](#q4-5)


<div id="question-1"/>

### I. What is State in react app?

For example, some common state in the whole app (might in multiple components):
- user authenticated state
- modal open state
- some list of data state
- ...

Why do we need another package (Redux) to manage state?
- state can be complex
- need a global variable to share through entire application that we can access from anywhere
- redux provide a central place and the way to manage data, which can be **easily integrate into React App**.

Note:
Redux is a theoretically independent 

<div id="question-2"/>

### II. Understand Redux Data Flow

- Component
- Action: pre-defined info
- Reducers: receive action and update state. Note: ~~**no side effects, no async code**~~
- Central Store:  store **entire application state**
- (Automatic) Subscription: pass updated state as props to component

Refer to the following flow chart:

![image](../assets/reduxflow.png ':size=581x296')

<div id="question-3"/>

### III. Set up Redux in React project

<div id="q3-1"/>

#### 3.1 Import Redux to application

Install redux: [docs](https://redux.js.org/introduction/installation)

Command line: 
```
npm install redux
npm  install react-redux
```

JS code to import redux:
```js
const redux = require('redux');
/* or */
import { createStore } from 'redux';
```

<div id="q3-2"/>

#### 3.2 set up Reducer and Store

Note: 
- the Store must be **initialized with a "Reducer"**
- the reducer is strongly connected to the store/state

**1 ) Create the Reducer:**
here for example, just create a pretty simple reducer:
```js
// Reducer
const rootReducer = (state,action)=>{
	return state;
}
```

**2 ) Create the Store with Reducer:**
```js
const createStore = redux.createStore;
// Store
const store = createStore(rootReducer);
console.log(store.getState());
```


**3 ) Optimized: add Initial State:**
Create the initialState value to your redux store, to avoid undefined state at first.
```js
const initialState = {
	count: 0;
}
```
Then we need to update the rootReducer with this initialState value:
```js
// Reducer
const rootReducer = (state = initialState,action)=>{
	return state;
}
```

<div id="q3-3"/>

#### 3.3 dispatch Actions

**Syntax:**
call the dispatch function: [store.dispatch()](https://redux.js.org/api/store#dispatchaction)

**Convention to define an Action:**
- **must have** an `type` property
- you can also pass other optional payload fields 

For example, an action to increase or decrease the count value by one: 
```js
store.dispatch({type: 'INCREASE', value: 1});
store.dispatch({type: 'DECREASE', value: 1});
```

**Add corresponding  logic in Reducer:**

Due to each action type, we need to add logics in reducer to make it work.  Note: here we can access other payloads we passed in actions to current reducer logic, eg: `action.value` (here is 10).

Then we can update the reducer code as below:
```js
const rootReducer = (state = initialState,action)=>{
	if(action.type === 'INCREASE')
	{
		return {
			...state, 
			count: state.count + 10
		};
	}
	else if(action.type === 'DECREASE')
	{
		return {
			...state, 
			count: state.count - action.value
		};
	}
	return state;
}
```

<div id="q3-4"/>

####  3.4 Add Subscriptions

**Why:** 
- avoid manually call `store.getState()` everytime
- need automatically handlers **whenever store updates/changes** 

**Syntax:**
 [store.subscribe( callback function )](https://redux.js.org/api/store#subscribelistener)

Code example:
```js
// Subscription
store.subscribe(()=>{
	// handler for changes or updates
	console.log("subscription:", store.getState());
})
```

<div id="question-4"/>

### IV. Connecting Store to the React App

<div id="q4-1"/>

#### 4.1 Install react-redux package
```bash
npm install --save react-redux
```

<div id="q4-2"/>

#### 4.2 Import the Provider
docs: [Provider](https://react-redux.js.org/api/provider)
```js
import { Provider } from 'react-redux';
```

Then, wrap our app with Provider
```jsx
const store = createStore(reducer);
ReactDOM.render(
	<Provider store={store}><App /></Provider>,
	document.getElementById('root')
);
```

<div id="q4-3"/>

#### 4.3 Connecting Store to the react Component

**Syntax:** [connect()](https://react-redux.js.org/api/connect)

```js
import { connect } from 'react-redux';
```
- a function
- it returns a new, connected component that wraps your original component
- parameters in this connect function:
	- `mapStateToProps?: Function`
	-  `mapDispatchToProps?: Function | Object`
	- `mergeProps?: Function`
	- `options?: Object`

<div id="q4-4"/>

#### 4.4 **[mapStateToProps](mapStateToProps):**
To access your state object and values, you can define to **request what/which state fields** interested in this component.

For example:
```js
cosnt mapStateToProps = state => {
	return {
		cnt: state.count;
	}
}
export default connect(mapStateToProps)(MyComponent);
```

<div id="q4-5"/>

#### 4.5 How to Dispatch Actions in Component

Define the [mapDispatchToProps](https://react-redux.js.org/using-react-redux/connect-mapdispatch) function, and then pass to the `connect()` function.

Note: if you have no `mapStateToProps` as first parameter, just pass **null** as first parameter in `connect()`.

For example:
```js
const mapDispatchToProps = dispatch => {
	return {
		onIncreaseCount: () => dispatch({type: 'INCREASE'}),
		onDecreaseCount: () => dispatch({type: 'DECREASE'})
 	}
}
export default connect(null, mapDispatchToProps)(MyComponent);
```

Then you can use the functions `onIncreaseCount()` and `onDecreaseCount()` in your react component.

Usage Example:
```jsx
// functional component
<button onClick={props.onIncreaseCount}>Increase</button> 

// class component
<button onClick={this.props.onIncreaseCount}>Increase</button> 
```





