## React day03 - Context
 
I. [When to use Context](#p1)  

II. [Create Context ](#p2)  

III. [Context Provider](#p3)

IV. [Read the Context](#p4) 
- [useContext](#p4-1)   
- [Context.Consumer Wrapper](#p4-2)   

V. [Source Code](#p5)

<div id="p1" />  

### I. When to use Context

- **share data** that can be considered “**global**” for a tree of React components, eg: the current authenticated user, theme, or preferred language
- If you only want to avoid passing some props through many levels,  [component composition](https://reactjs.org/docs/composition-vs-inheritance.html)  is often a simpler solution than context.
	Example: render the component through props passed down
	```
	<ParentComponent child1={ChildComponent1} />
	/* in nested children component */
	{
		props.child1
    }
	```

<div id="p2" />  

### II. Create Context

**Syntax:**  [React.createContext](https://reactjs.org/docs/context.html#reactcreatecontext)

**Example:**
```js
const  AuthContext = React.createContext({
	isAuthenticated:  false,
	toggleLogin: () => { }
});
```

**Tip:**
- The `defaultValue` argument is **only** used when a component **does not have a matching Provider** above it in the tree.
- This can be helpful for testing components in isolation without wrapping them. 
- passing  `undefined`  as a Provider value does not cause consuming components to use  `defaultValue`.

<div id="p3" />  

### III. Context Provider

**Syntax:**  [Context.Provider](https://reactjs.org/docs/context.html#contextprovider)

**Example:**
```jsx
const  AuthContextProvider = props  => {
	const [isAuthenticated, setIsAuthenticated] = useState(false);
	const  toggleLoginHandler = () => {
		setIsAuthenticated(!isAuthenticated);
	}
	return (
		<AuthContext.Provider  value={{ isAuthenticated, toggleLogin:  toggleLoginHandler }}>
		{props.children}
		</AuthContext.Provider>
		)
}
export  default  AuthContextProvider;
```

**Tip:**
- can manage some stateful variable in global context
- can create your own custom Provider with more stateful logic

<div id="p4" />  

### IV. Read the Context 

<div id="p4-1" />  

#### 1 ) useContext() Hook

**Syntax:** [useContext(MyContext)](https://reactjs.org/docs/hooks-reference.html#usecontext)

**Example:**
```jsx
const  authContext = useContext(AuthContext)
/* read the value */
const  buttonText = authContext.isAuthenticated ? "Sign In" : "Sign Up";
return (
	<div>
		/* ...  */
		<button  onClick={authContext.toggleLogin}>{buttonText}</button>
	</div>
)
```

**Tip:**
- it still needs a `Context.Provider` top of parent component
- it allows you to **read and subscribe the changes** of the context

<div id="p4-2" />  

#### 2 ) Context.Consumer Wrapper

**Syntax:** [docs](https://reactjs.org/docs/context.html#contextconsumer)
```
<MyContext.Consumer>
  {value => /* render something based on the context value */}
</MyContext.Consumer>
```

**Example:**
```jsx
return (
<AuthContext.Consumer>
{ context  => (
	<div>
		<button  onClick={context.toggleLogin}>
		{context.isAuthenticated ? "Sign In" : "Sign Up"}
		</button>
	</div> 
	)
}
</AuthContext.Consumer>
)
```

**Tips:**
- Requires a [function as a child](https://reactjs.org/docs/render-props.html#using-props-other-than-render). 
- the "context value" here is equal to the value in the props of the Provider
- If there is no Provider for this context above, the `value` argument will be equal to the `defaultValue` that was passed to `createContext()`.


<div id="p5" />  

### V. Source Code

[github link](https://github.com/jialihan/React-features/tree/main/03-useContext)
