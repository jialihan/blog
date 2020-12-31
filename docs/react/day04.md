## React day04 - Redux: Basics

#### I. [What is State?](#question-1)

#### II. [Understand Redux Data Flow](#question-2)

#### III. [Set up Redux in React project](#question-3)
 - [Import Redux to application](#q3-1)
 - [Set up Reducer and Store](#q3-2)
 - [Dispatch Actions](#q3-3)
 - [Add Subscriptions](#q3-4)

#### IV. [Connecting Store to the React App](#question-4)
- [Install react-redux package](#q4-1)
- [Import the Provider](#q4-2)
- [Connecting Store to the react Component](#q4-3)
- [mapStateToProps](#q4-4)
- [mapDispatchToProps](#q4-5)

#### V. [More features](#question-5)
- [Pass & Get data with Action](#q5-1)
- [Switch-Case in Reducer](#q5-2)
- [Immutable Operations on State/Object](#q5-3)
- [Immutable Operations on Array](#q5-4)
- [Immutable Update Patterns](#q5-5)
- [Good Practice: Outsourcing Action Types](#q5-6)
- [Combine multiple Reducers](#q5-7)
- [Understand State Types](#q5-8)


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

<div id="q3-5" />

#### 3.5 source code for set up redux (basics)

github link: [redux-basics.js](https://github.com/jialihan/React-features/blob/main/04-redux/redux-basics/redux-basics.js)

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

<div id="question-5"/>

### V. More Features

<div id="q5-1"/>

#### 5.1  pass & get data with Action

Define any optional fields when you dispatch an action. Besides the `type` property, you can define any customized fields with any name you want.

For example: Pass Data to Action
```js
dispatch({
	type: 'INCREASE', 
	value: 1, 
	name:'', 
	payload:{}
})
```

Get data from the action in our reducer logic, you will receive the default **"action"** as props in your reducer, then get the corresponding property field defined by yourself, eg: `action.name`,  `action.value`  and  `action.payload`

For example: Get Data from Action
```js
const rootReducer = (state = initialState,action)=>{
	if(action.type === 'INCREASE')
	{
		return {
			...state, 
			count: state.count + action.value,
			name: action.name,
			payload: action.payload
		};
	}
	return state;
}
```

<div id="q5-2"/>

#### 5.2  Switch-Case in Reducer

Syntax: switch by `action.type`
For example:
```js
const rootReducer = (state = initialState,action)=>{
	switch (action.type)
	{
		case 'INCREASE':
			return {
				...state,
				count: state.count + 1
			}
		case 'DECREASE':
			return {
				...state,
				count: state.count - 1
			}
	}
	return state;
}
```

<div id="q5-3"/>

#### 5.3  Immutable Operations on State/Object

Always return a new object as the new state:
- [Object.assign](As%20mentioned%20above,%20Object.assign%28%29%20will%20do%20a%20shallow%20clone,%20fail%20to%20copy%20the%20source%20object%27s%20custom%20methods,%20and%20fail%20to%20copy%20properties%20with%20enumerable:%20false.)({}, state):
	- does a **shallow copy**
	- enumerable own properties: ~~no customized method~~
	- fail to copy properties with `enumerable: false`.
	- Note: 
	For the purposes of redux, `Object.assign()` is sufficient 	 because the state of a redux app only contains immutable values (JSON).
- spread operator



For example:
```js
const newState = Object.assign({}, state);
const newState = {...state};
```

<div id="q5-4"/>

#### 5.4 Immutable Operations on Array

**Add elements into Array:**
[Array.concat()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/concat): it always return a new object array as result.

For example:
```js
const newArray = array1.concat(array2);
```

**Delete elements in Array:**

- ~~Array.splice(index, count):~~ in existing array, mutable, **wrong way!!!**
- Correct Way: spread operate to create a new array, then `splice()` to delete
- Correct Way2: [Array.filter(function)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter) 
	Syntax:
	```js
	array.filter((el, index)=>{
		return true/false;
	})
	```
	For example: 
	```js
	// delete the item with certain id
	results.filter( (el, index) => index!==id );
	```

<div id="q5-5"/>

#### 5.5 Immutable Update Patterns
Docs: 
https://redux.js.org/recipes/structuring-reducers/immutable-update-patterns/

**Correct Approach: Copying All Levels of Nested Data**

Here's what an example of updating `state.first.second[someId].fourth` might look like:
```
function updateVeryNestedField(state, action)  {
	return  {
		...state,
		first :  {
			...state.first,
			second :  {
				...state.first.second,
				[action.someId]  :  {
					...state.first.second[action.someId],
					fourth : action.someValue
				}
			}
		}
	}
}
```

<div id="q5-6"/>

#### 5.6 Outsourcing Action Types

**Good Practice:** 
store the name strings of the action into **constants** and export them.

create a "action.js" file, and put all action name strings here:
```js
// action.js
export const INCREASE = 'INCREASE';
export const DECREASE = 'DECREASE';
```

Use these constants:
```js
// reducer.js
import * as actionTypes from 'action.js';
/* ... */
switch (action.type)
{
	case actionTypes.INCREASE:
		return { ... };
}
```

<div id="q5-7"/>

#### 5.7 Combine multiple Reducers

When the application grows larger, it's a good practice to combine multiple reducers into one.

Syntax:  [combineReducers()](https://redux.js.org/api/combinereducers)
```jsx
import Reducer1 from './store/reducers/reducer1.js';
import Reducer2 from './store/reducers/reducer2.js';
import { createStore, combineReducers } from 'redux';
const rootReducer = combineReducers({
	rd1: Reducer1,
	rd2: Reducer2
});
```

Note: 
here the splitted properties `rd1, rd2` also split the global state into two parts. To access the state, use `state.rd1.value1`, and `state.rd2.value2`.

<div id="q5-8"/>

#### 5.8 Understand State Types

Do we need put every state into redux store? Which state should be managed by redux?

Note:  everytime user closed the browser or refresh, everything is gone in redux and it's not a replacement of database.

| Type | Example |  Use Redux? |
|--|--|--|
| Local UI State | Show/Hide Backdrop | mostly handled within components |
| Persistent State | All Users, all Posts, ... | Store on Server, relevant slice managed by redux |
| Client State | is authenticated? filters set by User | Managed via Redux|
 
 





