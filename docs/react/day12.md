## React Day12 - React Router & Routing

#### I. [Background & Intro](#question-1)
  
#### II. [What is React Router?](#question-2)
- [`<BrowserRouter>` component](#q2-1)
- [`<Route>` component](#q2-2)
- [`<Switch>` component](#q2-3)

#### III. [Library: React Router Dom ](#question-3)
- [`match` property](#q3-1)
- [`<Link>` component](#q3-2)
- [`history` property](#q3-3)
- [`location` property](#q3-4)

#### VI. [ `withRouter` HOC ](#question-4)

#### V. [ source code  ](#question-5)
 

<div  id="question-1"  />

### I. Background & Intro

| React | Angular |
|--|--|
|  a javascript library |  a framework |
|  didn't provide routing |  with pre-built routing |

Important two things for routing in Single Page Application:
- The standard react routing library: **DOC**: [react-router](https://reactrouter.com/web/guides/quick-start)

- key API: **history api** provide by react-router

- pacakge: `npm install react-router-dom`

<div  id="question-2"  />

### II. What is React Router?
  
  <div  id="q2-1"  />
  
#### 2.1 BrowserRouter Component
Import: 
```js
import { BrowserRouter } from  'react-router-dom';
```
Usage: 
- in "index.js", wrap our whole app
- it gives the children **all the functionalities of routing** this library provides.
```jsx
ReactDOM.render(
	<BrowserRouter>
	<App  />
	</BrowserRouter>,
	document.getElementById('root')
);
```

  <div  id="q2-2"  />
  
#### 2.2 Route Component 

Key properties:
- exact: whether the path has to be an exact match
	For example: `localhost:3000/` and `localhost:3000/profile`
	- exact:  **true** - two different url
	- excat: **false** - `'/'` path will also render if enter `'/profile'` path.
- path: eg: the base url  `'/'`
- component: react component you want to navigate

Code example:
```jsx
import { Route, Switch } from  'react-router-dom';
function  App() {
	return (
		<div>
			<Route  exact  path='/'  component={HomePage}  />
			<Route  exact  path='/profile'  component={ProfilePage}  />
		</div>
	);
}
```

  <div  id="q2-3"  />
  
#### 2.3 Switch Component
**Syntax:**
Wrap all our `<Route />` components inside of the `<Switch>` component.  
```jsx
import { Route, Switch } from  'react-router-dom';
function  App() {
	return (
		<div>
			<Switch>
				<Route  path='/'  component={HomePage}  />
				<Route  path='/profile'  component={ProfilePage}  />
			</Switch>
		</div>
	);
}
```
**Usage:**
Whenever the **exact path existed** inside the Switch, It only renders that route and nothing else, even if multiple paths are valid because of the `exact` keyword. So the **order of your code** is important to decide which path might be search at first.

Example: when user enter "/profile" on url
- path1: `'/'`
- path2: `'/profile'`

Without Switch:  both components rendered
With Switch: only ProfilePage rendered


<div  id="question-3"  />

### III. Library: react-router-dom

Routing Library: [react-router-dom](https://reactrouter.com/web/guides/quick-start)
 routing properties  in Route component :
 - match
 - history
 - `<Link>`
 - location


  <div  id="q3-1"  />
  
#### 3.1  `match` property

![image](../assets/matchproperty.png ':size=345x149')

- `path`: the url pattern that matched from component to the `<Route path="/" />`
- `isExact`: if the **whole UR**L is matched, then **true**.
- `params`:  dynamic params passed from the URL
	For example:
	```jsx
	path: '/profile/:profileId'
	// usage
	const  ProfileDetailPage = (props) => {
		return (<div>
				<h1>Profile Detail Page: {props.match.params.profileId}</h1>
			</div>)
	}
	```
- `url`: get the current matched url path, and support **nested routing under current matched url path**, **when component doesn't know the entire URL.**
	For example: 
	current match URL: `props.match.url`
	```jsx
	<Link  to={`${props.match.url}/1`}>To Profile 1 </Link>
	<Link  to={`${props.match.url}/2`}>To Profile 2 </Link>
	<Link  to={`${props.match.url}/3`}>To Profile 3 </Link>
	```
	
<div  id="q3-2"  />

#### 3.2 Link Component
**Docs:**  [Link component](https://reactrouter.com/web/api/Link)

**Import:**
```js
import { Route, Link } from  'react-router-dom';
```

**Usage:**
A simple link example, it can have more attributes for the Link Component, please reference to the Docs.
```jsx
const  HomePage = (props) => {
return ( <div>
	<Link to='/profile'> Profile</Link>
	<h1>Home Page</h1>
</div>
)}
```
**Notes:**
- Link force to tell react **which component to re-render**
- Link tells react where to redirect

<div  id="q3-3"  />

#### 3.3  `history` property
Docs: [history](https://reactrouter.com/web/api/history)

![image](../assets/historyproperty.png ':size=544x184')

- `push` property
	defined re-direct actions:
	```jsx
	<button onClick={() => {
		props.history.push('/profile')
		}}
	>navigate to Profile
	</button>
	```

<div  id="q3-4"  />

#### 3.4  `location` property

Docs: [location](https://reactrouter.com/web/api/location)

![image](../assets/locationproperty.png ':size=337x142')

It tells where we are currently, what is our application.
- `pathname`: the **full path** name that our **current URL**
	
<div  id="question-4"  />
 
### IV. `withRouter` Component

#### 4.1 Bad pattern: tunneling history props

we pass the `history` props down to all children, in order to get some info from `history`  routing property. But not every middle components really needs the `history` info.


#### 4.2 "withRouter" Component

Docs: [withRouter()](https://reactrouter.com/web/api/withRouter)

It's a HOC from 'react-router-dom', a function takes a component as argument and produce a new component.

**Code example:**
```jsx
import { withRouter } from  'react-router-dom';

const  MenuItem = ({ title, linkUrl, history, match }) => (
	<div  
		style={{ width:  '100px', height:  '100px', border:  '1px solid blue' }}
		onClick={() =>  history.push(`${match.url}profile/${linkUrl}`)}
	>
		<h1  className='title'>{title}</h1>
	</div  >
);
export default withRouter(MenuItem);
```

<div  id="question-5"  />
 
### V. source code

[github link](https://github.com/jialihan/React-features/blob/main/07-react-router/react-router-example/src/App.js)
