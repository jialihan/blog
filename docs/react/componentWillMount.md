# Safely use setState() in componentWillMount

## Use Case

Before the render(), I try to do something complicated to set some initial states for the component, but doesn't contain any async calls. So I need to call setSate() in the componentWillMount() life cycle, before the dom rendered. My goal is similar logic below, but to build in functional components using hooks.

## Class-based Component

Due to the official React docs([Link]([https://reactjs.org/docs/react-component.html#unsafe_componentwillmount](https://reactjs.org/docs/react-component.html#unsafe_componentwillmount))):
`UNSAFE_componentWillMount()` is invoked just before mounting occurs. It is called before `render()`, therefore <mark>calling `setState()` synchronously in this method will not trigger an extra rendering.</mark> Generally, we recommend using the `constructor()` instead for initializing state.
So, we can have temporary solution in class-based component to call setState() synchronously like this:
```
componentWillMount()
{
	// Todo something sync
	this.setState({......});
}
```

## Functional Component

###  Problem 1: how to implement componentWillMount() in React hook
It's familiar with us that `useEffect()` can be used equivalently as combination or singly like `componentDidMount, componentDidUpdate,componentWillUnMount`, but we need to write a function specially to mock componentWillMount() to do something before rendered.
Reference([Link]([https://stackoverflow.com/questions/53464595/how-to-use-componentwillmount-in-react-hooks](https://stackoverflow.com/questions/53464595/how-to-use-componentwillmount-in-react-hooks))):
### componentDidMount hook
```
 const useComponentDidMount = func => useEffect(func, []);
```
### useComponentWillMount

Because functional component doesn't have a equivelant constructor, using a hook to run code before component mounts might make sense in certain cases. Here we build a custom hook to help.
```
const useComponentWillMount = func => {
  const willMount = useRef(true);
  if (willMount.current) {
    func();
  }
  willMount.current = false;
};
```
### Problem 2: setState() in custom hook will trigger re-render()
The problem is that, if we call `setState()` in the func() to execute before the render like this:
```
const [someState,setSomeState] = useState();
useComponentWillMount(() => {
	// Todo something sync
	setSomeState({...});  // here will trigger multiple re-renders
 });
```
we got the following error:
```diff
- Too many re-renders. React limits the number of renders to prevent an infinite loop.
``` 

### Solution: for functional component
To avoid using custom hook to call setState(), we try to do those things in constructor. But functional component doesn't have a constructor() function for us to safely to call once. Try to use `Lazy initial state`([Link]([https://reactjs.org/docs/hooks-reference.html#lazy-initial-state](https://reactjs.org/docs/hooks-reference.html#lazy-initial-state))):
If the initial state is the result of an expensive computation, you may provide a function instead, which will be executed only on the initial render:
```
const [state, setState] = useState(() => {
  const initialState = doSomethingBeforeRender(props);
  return initialState;
});
```


## Summary

1. Avoid to use componentWillMount(), try to do something and init in `constructor` if you use a class-based component, and use `lazy initial state` if you use a functional component.
2. Pay attention to that setState() may also be asynchronous, because  `this.props`  and  `this.state`  may be updated asynchronously, we should not rely on their values for calculating the next state. setState() also accept a function beside an object, so the correct way to pass it a function instead of an object, both arrow func and regular function are good:
	```
	this.setState((state, props) => ({
		counter: state.counter + props.increment // increment is async func
	}));
	```