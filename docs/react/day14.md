## React Day14 - LocalStorage & SessionStorage & Redux Persist

#### I. [Problem](#question-1)
  
#### II. [Local Storage](#question-2)

#### III. [Session Storage](#question-3)

#### IV. [Redux Persist](#question-4)
- [ Install](#q4-1)
- [ Setup  persistStore](#q4-2)
- [ Setup persistReducer](#q4-3)
- [Setup PersistGate](#q4-4)

<div  id="question-1"  />

### I. Problem
When user refresh the page, even the data in r**edux store will lose and reset**, in a brand new state.

<div  id="question-2"  />

### II. Local Storage

Docs: [local storage - MDN](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage)

In our window object, we have access to local storage:
```js
window.localStorage
```

Persist:
Local Storage will persist until we clear it out, even if we close our window & close our browser, it still there.

API:
- `getItem(key_string)`
- `setItem(key_string, value_string)`
- `clear()`

For example:
```js
// set
const myObject = {name: 'jelly'};
window.localStorage.setItem('myItem', JSON.stringify(myObject));
// get
const receivedValue = window.localStorage.getItem('myItem');
const receivedObj = JSON.parse(receivedValue);
```

<div  id="question-3"  />

### III. Session Storage

Docs: [sessionStorage - MDN](https://developer.mozilla.org/en-US/docs/Web/API/Window/sessionStorage)

In our window object, we have access to session storage:
```js
window.sessionStorage
```

Persist:
When user refresh the page, **session still there until we close our tab in browser.**

API
- `setItem('key_string', 'value_stirng')`
- `getItem('key_string')`
- `removeItem('key_string')`
- `clear()`

  
<div  id="question-4"  />
 
### IV. Redux Persist

Docs: [redux-persist github](https://github.com/rt2zz/redux-persist#readme)
- It let us leverage either local storage or session storage easily

<div  id="q4-1"  />

#### 4.1 Install:
```bash
npm install redux-persist
```

<div  id="q4-2"  />

#### 4.2 Setup  persistStore
It allows our browser to **cache the store** depending on certain configuration.

**Import:**
```js
import { persistStore } from 'redux-persist';
```

**Usage:**
We will use the persister & store to wrap our application.
-  `export:  store`
-  `export:  let  persistor = persistStore(store)`

<div  id="q4-3"  />

#### 4.3 Setup persistReducer

**Import:**
- persistReducer
	```js
	import { persistReducer } from 'redux-persist';
	```
-   **localStorage**  `import storage from 'redux-persist/lib/storage'`
-   **sessionStorage**  `import storageSession from 'redux-persist/lib/storage/session'`

**Define Persist Config:**
```js
// BLACKLIST
const persistConfig = {
  key: 'root',
  storage: storage,
  blacklist: ['navigation'] // navigation will not be persisted
};

// WHITELIST
const persistConfig = {
  key: 'root',
  storage: storage,
  whitelist: ['navigation'] // only navigation will be persisted
};
```

**Usage:** 
we need to call the reducer, wrap our rootReducer inside of the persistReducer. 
```js
export default persistReducer(persistConfig, rootReducer);
```

<div  id="q4-4"  />

#### 4.4 Setup PersistGate

Then use this new wrapped reducer as our app's reducer, to wrap your root component with [PersistGate](https://github.com/rt2zz/redux-persist/blob/master/docs/PersistGate.md). This delays the rendering of your app's UI until your persisted state has been retrieved and saved to redux. 
- **NOTE:** the `PersistGate` loading prop can be null, or any react instance, e.g. `loading={<Loading />}`

**Import:**
```js
import { PersistGate } from 'redux-persist/integration/react';
import {store, persistor} from './redux/store';
```

**Usage:**
```jsx
const App = () => {
  return (
    <Provider store={store}>
      <PersistGate loading={null} persistor={persistor}>
        <App />
      </PersistGate>
    </Provider>
  );
};
```

So finally, we can maintain the state of items & data in redux store. it will **persist and rehydrate** these state.



