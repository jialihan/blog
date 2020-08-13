## React Hooks

### 1. what is react hooks?
Attention:
* Do **not** confuse with class-based **life-cycle hooks** ! They are different!
* It's only for **functional component**  or **other hooks**to use!
* React hooks are just **javascript functions**!
* Naming rule: **useXXX()** -> lowercase use with your hook name in camel case

The purpose/goal/idea to use hooks:
* Hooks are highly **Reusable**: 
we can use the **same hook** in **multiple** functional components or other hooks and the usage of it is **Independent** from the other places.
* it can simply **share functionality** between components, but not sharing the data.
* Capability of react hooks:
 
    - allow you to `add state` to functional components 
    - you can `share logic across components` with hooks and that` logic can even be stateful`
    - **But:** you actually `can't replace lifecycle methods` with hooks

### 2. Hooks Rules: important!
Two important rules to use react hooks:
*  must use hooks in `functional components` or other custom built hooks !
*  `can't` use a hook in some `nested function`.

### 3. useState() hook
3.1 ) Import:
```
import React, { useState } from 'react';
```
3.2 ) usage:
```
// destructing 
const  [ myState, setMyState]  =  useState('');
// use as array
const mystate = useState('');
mystate[0]
mystate[1]({new state});
```
* The initial state can be anything: can be an` object, an array, a number, string,  boolean`, it can be any value.
* It always return 2 parameter: 
   - first one: current state
   - second one: function to call with your new state value to set state
 * setState will `not merge new state`, be careful to not lose your old state.
   We can retrive the previous state, for example: `pass a function as param to setState()`:
   ```
	setMyState(prevState  => ({
		title: prevState.title
	}));
   ```

  3.3 ) passing state data between components
  Wrap state data in eventHandler, then pass `function reference` to child components. For example:
  ```
  const onButtonClickedHandler = ()=>{
	setMyState(false);
  }
  <MyChildComponent onBtnClicked={onButtonClickedHandler} />
  ``` 
 In child component how to use and call this function? 
Then in our child component:
```
// MyChildComponent.js
props.onBtnClicked();
```
  3.4 )  Reference doc: [use state hook](https://reactjs.org/docs/hooks-state.html)

### 4. useEffect hook
#### 4.1)  what is useEffect function?
The  _Effect Hook_  lets you perform side effects in function components.
FYI: 
 **Side effects** are basically anything that **affects** something outside of the scope of the current **function** that's being executed. For example: API requests to our backend service.
Syntax:
```
import React, { useEffect } from 'react';
useEffect(() => {    
	// todo
});
```
#### 4.2) when it will execute?
That's important `by default`, it gets executed `right after important after every component render a cycle`.

#### 4.3) Second Parameter
it controls when to trigger re-render cycle. Usually it summarize as three common use case:
* **None** second parameter
   it executes including the **first render** and after that for **every render cycle**
   ```
   useEffect(() => {    
  });
   ```
* Use  **empty array**  `[]` as sencond parameter
   only execute `once for initial render`, similar to the functionality of life-cycle hook `componentDidMount`.
    ```
  useEffect(() => {    
  }, []);
   ```
* Use **array with dependencies** `[props]` as second parameter.
    ```
  useEffect(() => {    
  }, [props.title]); // only when title changes
   ```
#### 4.4) clean up
It clean up that previous effect before run a new one. It also have 3 use case due to the second param in _useEffect()_ hooks.
* No second param
	```
	useEffect(()  =>  {
	return  cleanUp  =  ()  =>  {
		 // run before re-render
	     // and before final unmount
	}
	});
	```
* empty array `[]` as second param
	```
	useEffect(()  =>  {
	return  cleanUp  =  ()  =>  {
		// only run once before this component unmount
	}
	}, []);
	```
* array with dependency  `[props]` as second param
	```
	useEffect(()  =>  {
	return  cleanUp  =  ()  =>  {
		// run before every re-render cycle
		// and before this component unmount
	}
	}, [prop1, prop2, prop3]);
	```
#### 4.5)  Reference link:   
   [useEffect Hook](https://reactjs.org/docs/hooks-effect.html)

### 5. useRef() Hook
5.1 ) purpose/goal/capability
This allows us to create a reference simply by calling `useRef`. And it helps to  **establish a connection between DOM elements and the reference variable** in our code.

5.2)  Look back how we use it in class-based component
The key point is to use `React.createRef()` function to store the reference in a variable. And we assign this reference to our DOM element with `ref attribute`.
```
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    // create ref
    this.myRef = React.createRef();  
  }
  render() {
    // point ref to DOM Element
    return <div ref={this.myRef} />;  }
}
```

5.3) useRef hook in  functional component
* create the ref:
	```
	const myRef = useRef(initialValue);
	```
	`useRef`  returns a mutable ref object whose  `.current`  property is initialized to the passed argument (`initialValue`).
* access the ref object: use `.current`
	```
	myRef.current.focus(); // for example
	```
* connect to DOM Element: use `ref` attribute
	```
	<input ref={myRef} />
	```

5.4) Reference doc link: [useRef hook](https://reactjs.org/docs/hooks-reference.html#useref)

### 6.  useReducer Hook
6.1) when to use `useReducer()`?
 * when you have complex state logic that involves multiple sub-values 
 * when the next state depends on the previous one. 

6.2) Syntax
```
const [state, dispatch] = useReducer(reducer, initialArg, init);
```
Accepts a reducer of type  `(state, action) => newState`, and returns the current state paired with a  `dispatch`  method. 

6.3) Initial State
There are two ways to define and init the state.
* Simple way: pass state object at the very beginning
	```
	 const [state, dispatch] = useReducer(
	    reducer,
	    {...initialSate}  
	 );
	```
* **Lazy Init**
Why we use lazy init?
  - It lets you extract the logic for `calculating the initial` state outside the reducer.
  - This is also helpful for `resetting the state later` in response to an action.

Code Example: define a function to calculate initial state outside:
```
function init(initialCount)
{  
	// Do Calculations here
	return your_computed_state_object;
}
```

6.3) **how to create a reducer function?**
For example: the below template to write a reducer function
```
function reducer(state, action) {
  switch (action.type) {
    case 'INC':
      return newState;
    case 'DEC':
      return newState;
    default:
      throw new Error();
  }
}
```

6.4 ) Reference api link: [useReducer hook](https://reactjs.org/docs/hooks-reference.html#usereducer)

### 7. useContext Hook
* <font size="5">Step1:  create a context object</font>
	```
	import React from  'react';
	const myContext = React.createContext(
	{
		key: value
	}
	);
	```
* <font size="5">step2:  create contextProvider</font>
```
export default const myContextProvider  =  props  =>  {
	return (
		<MyContext.Provider value={{ key : val }}>
			{props.children}
		</MyContext.Provider>
	);
};
```
**Attention:**  `value attribute` here is important for all its child component listening to. The value will `automatically be distributed to everyone listening` if if it `changes` which describes our context, then they will `get the updated value`.

* <font size="5">Step3:  Wrap our App with Provider</font>
```
ReactDOM.render(
	<AuthContextProvider>
		<App  />
	</AuthContextProvider>,
	document.getElementById('root')
);
```

* <font size="5">Step4: Child Consume the context</font>
	1 )  import our context object
	```
	import  { MyContext }  from  '../context/my-context';
	```
	2 )  use the context by `useContext() hook`
	```
	const myContext = useContext(MyContext);
	```

Reference api docs: [useContext Hook](https://reactjs.org/docs/hooks-reference.html#usecontext)

### 8. useCallback() Hook
8.1 ) what is `useCallback()` for?
it returns a  [memoized](https://en.wikipedia.org/wiki/Memoization)  callback function. Advantage is: it `prevents unnecessary renders`.

8.2) Syntax: 
```
const myCallback = useCallback(
 myFunction, [dependencies]
);
```
* **param1**: inline callback function
* **param2**: an array of dependencies.  can be empty
* **return**:   will return a memoized version of the callback that only changes if one of the dependencies has changed.

8.3) it equals to the `useMemo()` on your functions
```
useCallback(func, [dependencies]) 
===
useMemo(() => func, [dependencies])
```

8.4) Reference api link: [useCallback Hook](https://reactjs.org/docs/hooks-reference.html#usecallback)


### 9. useMemo() Hook
9.1) what is useMemo()
Attention: **it only returns a [memoized](https://en.wikipedia.org/wiki/Memoization) value.**
The reason/goal/purpose to useMemo() is:
This optimization helps to avoid expensive calculations on every render.

9.2) syntax:
```
const memoizedValue = useMemo(
	 () => computeExpensiveValue(a, b), 
	 [a, b]
 );
```

9.3) dependency array:
* **no array provided:**  a new value will be computed on every render.  
*  **array with dependency [props]:**
`useMemo` will only recompute the memoized value when one of the  dependencies has changed. 
* **empty array []:** has no dependency that means it will never change, so it will only computed once then store the initial value.

9.4) Reference api link: [useMemo Hook](https://reactjs.org/docs/hooks-reference.html#usememo)


### 10. Custom Hook
#### 10.1 ) what is custom hook?

A custom Hook is a JavaScript function staring with **"use"** and it helps to **share stateful logic** between components, and Each _call_ to a Hook gets **isolated state**.

#### 10.2 ) rules to create your own hook
*  name starts with `"use"` 
* you can call other Hooks inside your own hook
* you can decide what it takes as `arguments` and its `return`.

#### 10.3 ) create your hook function
For example, this template code from react official docs: [hooks-custom react](https://reactjs.org/docs/hooks-custom.html)
```
function useFriendStatus(friendID) {
  const [isOnline, setIsOnline] = useState(null);
  // ...
  return isOnline;
}
```
My another example, we can also use any other react hooks in our own hook. For example, the following `useHttp()` hook extract the dispatch logic to use http, you can use this hook to **get/post/delete/update** http request.
```
const  useHttp  =  ()  =>  {

	const  [ httpState, dispatchHTTP ]  =  useReducer(httpReducer, initialState);

	// fetch call to do something

	return  {
	data: httpState.data,
	};
};
```

#### 10.4) Use your own  hook
* For the useFriendStatus() example: [ React docs link](https://reactjs.org/docs/hooks-custom.html)
We can pass info as parameter to our own hook:
The  `useState`  Hook call gives us the latest value of the  `recipientID`  state variable, we can pass it to our custom  `useFriendStatus`  Hook as an argument:

```
  const [recipientID, setRecipientID] = useState(1);
  const isRecipientOnline = useFriendStatus(recipientID);
```
* For my own exmaple:
```
const  { data }  =  useHttp();
```

#### 10.5)  My source code example of using react custom hooks
[github: My own useHttp Hook ](https://github.com/jialihan/react-hooks-demo/blob/master/src/hooks/http.js)