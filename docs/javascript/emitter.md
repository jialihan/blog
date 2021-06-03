## Create an Event Emitter in JavaScript

#### I. [Problem and description](#p1)  

#### II. [Simple solution: Map + array](#p2)  

#### III. [Optimized: Nested Map solution](#p3)

<div id="p1" />

### I. Problem and description

**link:** [bfe 16 Emitter](https://bigfrontend.dev/problem/create-an-Event-Emitter)

There is  [Event Emitter in Node.js](https://nodejs.org/api/events.html#events_class_eventemitter), Facebook once had  [its own implementation](https://github.com/facebookarchive/emitter)  but now it is archived.
You are asked to create an Event Emitter Class
```js
const emitter = new Emitter()
```
It should support event subscribing
```js
const sub1  = emitter.subscribe('event1', callback1)
const sub2 = emitter.subscribe('event2', callback2)

// same callback could subscribe 
// on same event multiple times
const sub3 = emitter.subscribe('event1', callback1)
```
`emit(eventName, ...args)`  is used to trigger the callbacks, with args relayed
```js
emitter.emit('event1', 1, 2);
// callback1 will be called twice
```
Subscription returned by  `subscribe()`  has a  `release()`  method that could be used to unsubscribe
```js
sub1.release()
sub3.release()
// now even if we emit 'event1' again, 
// callback1 is not called anymore
```

<div id="p2" />

### II. Simple solution: Map + array
**Time Complexity:**
- subscribe: **O(1)** - using `map.get()`
- release: **O(n)** - using array to find the index of element

**Note:**
Bug: be careful to use the **arrow function** in returned object, since we need to access to `this` keyword, if you use **function declaration (wrong)** will cause error.

**JS code:**
```js
class EventEmitter {
  constructor(){
    this.map = new Map();
  }
  subscribe(eventName, callback) {
    if(!this.map.has(eventName)) {
      this.map.set(eventName, []);
    }
    this.map.get(eventName).push(callback);
    return {
      release: () => { // bug: must use arrow func to have access to the lexical scope for "this" keyword
        var index = this.map.get(eventName).indexOf(callback);
        if (index >= 0) {
            this.map.get(eventName).splice(index, 1);
        }
      }
    }
  }
  emit(eventName, ...args) {
    this.map.get(eventName).forEach(el=>{
      el.call(null,...args);
    });
  }
}
```

<div id="p3" />

### III. Optimized: Nested Map solution

**Optimized data structure:**  **<eventName, <callback, count>>**

**Time Complexity:**
- subscribe: **O(1)** - using `map.get()`
- release: **O(1)** - using `map.get()` and update count of the callback functions.

**JS code:**

```js
class EventEmitter {
  constructor(){
    this.map = new Map(); // <eventName, <callback, count>>
  }
  subscribe(eventName, callback) {
    if(!this.map.has(eventName)) {
      this.map.set(eventName, new Map());
    }
    var cntMap = this.map.get(eventName);
    cntMap.set(callback, (cntMap.get(callback) || 0) + 1);
    return {
      release: () => {
        var newCount = this.map.get(eventName).get(callback) - 1;
        if(newCount === 0)
        {
          cntMap.delete(callback);
        }
        else
        {
          cntMap.set(callback, newCount);
        }
      }
    }
  }
  emit(eventName, ...args) {
    if(!this.map.has(eventName))
    {
      return;
    }
    for (var [key, value] of this.map.get(eventName)) {
        for(var t = 0; t<value; t++) {
          key.call(this, ...args);
        }
    }
  }
}
```
