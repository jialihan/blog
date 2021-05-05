
## React day16: create-react-app tricks
 
I. [How to use absolute path imports?](#p1)  

II. [Check 'prop-types'](#p2)  

III. [Generate unique key in array of objects - UUID](#p3)

IV. [Bind onClick listener on List Item in React](#p4) 

V. [do NOT use "for" in label in React](#p5)

VI. [Source Code](#p6)

<div id="p1" />  

### I. When to use Context

**Reference article:** 
[stackoverflow-link](https://stackoverflow.com/questions/45213279/how-to-avoid-using-relative-path-imports-redux-action-action1-in-cre), [Absolute imports with Create React App](https://medium.com/@mikecripps/absolute-imports-with-create-react-app-d70fb65ea012)

**Issue:**
```
./src/App.js
Module not found: Can't resolve 'src/components/ChatBox' in '/Users/xxxxx/Documents/xxxxx/react-chat-app-socket_io/src'
```
**Solution:**
add the `jsconfig.file`, and add the following code:
```json
{
	"compilerOptions": {
		"baseUrl": "./"
	}
}
```
Then it supports the paths:
- `"src/components/...." `
- `"src/App.js"`
- .....


<div id="p2" />  

### II. Check 'prop-types'

**Install:**  [prop-types](https://www.npmjs.com/package/prop-types)
```bash
npm install --save prop-types
```

**Docs:** [Typechecking With PropTypes](https://reactjs.org/docs/typechecking-with-proptypes.html)

**Usage:**
```js
import  PropTypes  from  'prop-types';

optionalArray: PropTypes.array,
optionalBool: PropTypes.bool,
optionalFunc: PropTypes.func,
optionalNumber: PropTypes.number,
optionalObject: PropTypes.object, // deprecated
optionalObject2: PropTypes.shape({}),
optionalString: PropTypes.string,
optionalSymbol: PropTypes.symbol,
// An object taking on a particular shape
optionalObjectWithShape: PropTypes.shape({
	color: PropTypes.string,
    fontSize: PropTypes.number
}),
```

<div id="p3" />  

### III. Generate unique key in array of objects - UUID

**Docs:**  [uuid - npm](https://www.npmjs.com/package/uuid), [github - uuid](https://github.com/uuidjs/uuid)

**Install:**
```bash
npm install uuid
```

**Usage & example:**
```js
import { v4 as uuidv4 } from 'uuid';
uuidv4(); // ⇨ '9b1deb4d-3b7d-4bad-9bdd-2b0d7b3dcb6d'
```

<div id="p4" />  

### IV. Bind onClick listener on List Item in React

- bind the final `onClick={}` attribute on a native HTML tag, not a react component.
- NO event.target, need to bind

#### 4.1 class based component
```js
constructor(){
	this.handler = this.handler.bind(this);
}
handler(e)
{
	e.preventDefault();
	//
}
<li onClick={this.handler} ></li>
```

#### 4.2 functional component
Want to call with specific data in that `<li>` item, then, we have to `bind()` with specific **params**.
```js
const = handler(id, name, value)
{
	//
}
const ListItem = ({id, name, value, handler}) =>{
	return (
	<li onClick={handler.bind(null, id, name, value)} ></li>
	);
}
```

<div id="p5" />  

### V. do NOT use "for" in label in React


[stackoverflow -link](https://stackoverflow.com/posts/59924600/timeline)

When using React, you can't use the  `for`  keyword in JSX, since that's a javascript keyword (remember, JSX is javascript so words like  `for`  and  `class`  can't be used because they have some other special meaning!)

To circumvent this, React elements use  `htmlFor`  instead (see  [React docs](https://reactjs.org/docs/dom-elements.html#htmlfor)  for more information).

**For example:**
```js
render() {
    return (
        <div>
          <label htmlFor="username">name</label>
          <input type="text" id="username" value={this.state.blogs_name}  onChange={this.onChangeBlogsName} />
        </div>
    );
}
```

<div id="p6" />  

### VI. Source Code
// TODO