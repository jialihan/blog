## React-Day01 Custom Hooks

I. [Why use custom hooks?](#p1)

II. [Write a custom hook](#p2)

III. [Pass data between hooks](#p3)

<div id="p1" />

### I. why use custom hooks?

Docs: [hooks-custom](https://reactjs.org/docs/hooks-custom.html)

- share logic
- extract the logic code of "stateful"
- keep the UI component clean with less business logic code

<div id="p2" />

### II. write a custom hook

#### 1. Syntax

- create a functional component and naming starts with "**use**"
- return a array or object structure as a tuple

For example:
create a form custom hook to process the common **input** and `setSate()` logic.

```
export const useForm = (initialForm) => {

const [formState, setFormState] = useState(initialForm);

const inputChangedHandler = (event) => {
	const newState = {
	...formState,
	[event.target.name]: event.target.value,
	};
	setFormState(newState);
};

const resetForm = () => {
	setFormState(initialForm);
}

return [formState, inputChangedHandler, resetForm];
}
```

Source Code: [github link](https://github.com/jialihan/Custom-Hook-React/blob/main/src/hooks/useForm.js)

<div id="p3" />

### III. Pass data between Hooks

If we use a state variable as argument, our hooks will automatically unsubscribe the old value and execute on the new value.

From official docs:

```javascript
const [recipientID, setRecipientID] = useState(1);
const isRecipientOnline = useFriendStatus(recipientID);
```
