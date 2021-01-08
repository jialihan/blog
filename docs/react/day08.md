## React Day08 - Hooks in React-Redux
   
### I. [Why use hook in react-redux ?](#question-1)  

### II. [Two important hooks in react-redux ](#question-2)  

### III. [useSelector()](#question-3)

### IV. [useDispatch()](#question-4)

### V. [useStore() - don't use frequently](#question-5)

<div id="question-1" />

### I. Why use hook in react-redux ?

- In the past: we use `connect()` to **connect Component to Redux.**
- In react-redux version `7.1.0+`, it offers a set of hook APIs **as an alternative to the existing `connect()`** Higher Order Component.
- Docs:  [hooks in react-redux](https://react-redux.js.org/api/hooks)



<div id="question-2" />

### II. Two important hooks in react-redux

- [useSelector()](https://react-redux.js.org/api/hooks#useselector) :  get data from the redux store
- [useDispatch()](https://react-redux.js.org/api/hooks#usedispatch) : dispatch actions

<div id="question-3" />

### III. useSelector()

Goal:
Allows you to **extract data from the Redux store state**, using a selector function.

Docs:  [useSelector](https://react-redux.js.org/api/hooks#useselector)

#### 3.1 Syntax

This hook takes a function as an argument, which will give you the current state snapshot, then you return a value here.

**Note**: The selector function should be [pure](https://en.wikipedia.org/wiki/Pure_function) since it is potentially executed multiple times and at arbitrary points in time.

```js
const counter =  useSelector(state  => state.counter);
const lists = useSelector(state => state.lists);
```

#### 3.2 memorize the selector

**Why ?** 

When using `useSelector` with an inline selector as shown above, **a new instance of the selector is created whenever the component is rendered.**

**How** to create memorized selector ?

However, memoizing selectors (e.g. created via `createSelector` from `reselect`) do have internal state. 

**Docs:** [reselect library](https://github.com/reduxjs/reselect),  [createSelector](https://redux-toolkit.js.org/api/createSelector)

**Note:** 
- declare the selector instance **outside of component**, so that the same selector instance is used for each render.

**Code example:**
```js
const counterSelector =  createSelector(
	state  => state.items,
	items  => items.filter(item  => item.isDone)
)
export const MyComponent = () => {
	const myItems = useSelector(counterSelector)
	return  <div>{numOfDoneTodos}</div>
}
```

<div id="question-4" />

### IV. useDispatch()

This hook **returns a** reference to the `dispatch` **function** from the Redux store.

#### 4.1 Syntax:
Import:
```js
import  { useDispatch }  from  'react-redux'
```
Usage:
```js
const dispatch =  useDispatch()
const onIncreaseCount = () => dispatch(
	{ 
		type:  'INCREASE' 
	}
);
```

#### 4.2 useCallback to wrap the function of dispatch

when passing a callback using `dispatch` to a child component, you should memoize it with [`useCallback`](https://reactjs.org/docs/hooks-reference.html#usecallback).

You can safely pass `[dispatch]` in the dependency array for the `useCallback` call - since `dispatch` won't change.

Code example:
```js
const onIncreaseCountHandler =  useCallback( 
	() => dispatch({ type: 'INCREASE' }),
	[dispatch]
);
```

<div id="question-5" />

### V. useStore() - don't use frequently

This hook **returns a reference to the same Redux store** that was passed in to the  `<Provider>`  component.

Docs: [useStore()](https://react-redux.js.org/api/hooks#usestore)

Syntax:
```js
const store =  useStore()
```

**Note:**
- **This hook should probably not be used frequently**. 
	 Why? 
	Because the **component which uses this will not get updated.**
- Prefer  `useSelector()`  as your primary choice. 
- **Less common scenarios:**  be useful when it do require access to the store, such as replacing reducers.

Code example:
```js
// just a example, do not really use this code in your component
 const mystore = useStore();
 const countValue = mystore.getState().count;
 console.log(countValue);
```