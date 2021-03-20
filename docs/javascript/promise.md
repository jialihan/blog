## Native implementation of Promise
#### 1. [What is Promise?](#p1)  

#### 2. [Analysis Promise object structure](#p2)  
- [Three states](#p2-1)
- [Constructor](#p2-2)
- [Static methods](#p2-3)
- [Instance method](#p2-4)

#### 3. [Implementation](#p3)  

<div id="p1" />

### 1. What is Promise ?

Docs: [Promise object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)

**A promise is an object that may produce a single value some time in the future: either a resolved or rejected value.**

How event loop deal with promise & web api callback?
- macrotask queue: async web api callbacks, eg: setTimeout, setInterval......
- microtask queue: eg: Promise
	
In a event Loop, these two task queues will run in two steps:

1.  First, check whether there is a macrotask (called it X) in old macrotask queue ;
2.  If X exists and it is running, wait for gotoing the next step until it was complete; otherwise, goto the next step immediately;
3.  Second, run all microtasks of the microtask queue;
4.  and when run the microtasks, we can still add some more microtaks into the queue, these tasks will also run.

<div id="p2" />

### 2. Analysis Promise object structure

<div id="p2-1" />

#### 2.1 `Promise`  has three states:

-   _pending_: initial state, neither fulfilled nor rejected.
-   _fulfilled_: meaning that the operation was completed successfully.
-   _rejected_: meaning that the operation failed.

<div id="p2-2" />

#### 2.2 Constructor: [`Promise()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/Promise)

<div id="p2-3" />

#### 2.3  static methods
it has so many, define the scope, what is the major method I want to implement.
- [`Promise.all(iterable)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all)
- [`Promise.reject(reason)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/reject)
	Returns a new  `Promise`  object that is rejected with the given reason.
- [`Promise.resolve(value)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/resolve)
	Returns a new `Promise` object that is resolved with the given value. the value can be either:
	- thenable (eg: has a `then()` method)
	- a single value

<div id="p2-4" />

#### 2.4 instance method
- catch()
- then()
- finally()

<div id="p3" />

### 3. Implementation
Gaps:
- here only return `this` instead of a new promise object described in document for `then(). catch()` method.
- only implement a few major methods
- what's about `catch()` method doing?  [Promise.prototype.catch](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch)
- `then()` method can accept two arguments: `resolveHandler & rejectHandler`, [Promise.prototype.then](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/then)

```js
const PENDING = 0;
const FULFILLED = 1;
const REJECTED = 2;

class MyPromise {
    constructor(fn) {
        console.log("constructor...");
        // store state which can be PENDING, FULFILLED or REJECTED
        this.status = PENDING;
        // store value once FULFILLED or REJECTED
        this.result = null;
        this.resolveHandlers = [];
        this.rejectHandlers = [];

        this.onResolve = this.onResolve.bind(this);
        this.onReject = this.onReject.bind(this);

        fn(this.onResolve, this.onReject);
    }
    onResolve(value) {
        this.result = value;
        this.status = FULFILLED;
        let chainedValue = value;
        try {
            this.resolveHandlers.forEach(fn => {
                chainedValue = fn(chainedValue);
            });
        }
        catch (error) {
            this.resolveHandlers = [];
            this.onReject(error);
        }
    }
    onReject(value) {
        this.result = value;
        console.log("calling onReject...");
        this.status = REJECTED;
        let chainedValue = value;
        this.rejectHandlers.forEach(fn => {
            chainedValue = fn(chainedValue);
        });
        this.rejectHandlers = [];
    }
    then(handleResolved, handleRejected) {
        if (handleResolved) {
            if (this.status === FULFILLED) {
                console.log("then: already fulfilled")
                handleResolved(this.result);
            }
            else {
                this.resolveHandlers.push(handleResolved);
            }
        }
        if (handleRejected) {
            if (this.status === REJECTED) {
                console.log("then: already rejected")
                handleRejected(this.result);
            }
            else {
                this.rejectHandlers.push(handleRejected);
            }
        }

        return this; // self chaining
    }
    catch(handleError) {
        // It behaves the same as calling Promise.prototype.then(undefined, onRejected) 
        // (in fact, calling obj.catch(onRejected)
        // internally calls obj.then(undefined, onRejected)).
        this.then(undefined, handleError);
        return this; // self chaining
    }
}

// test
const p = new MyPromise((resolve, reject) => {
    setTimeout(function () {
        // resolve("Success!")
        reject("soemthing went wrong!");
    }, 2000)
});
p.then(resp => console.log(resp)).catch(e => console.log(e));

// some time later to monitor this promise
setTimeout(() => {
    p.then(resp => console.log(resp)).catch(e => console.log(e));
}, 3000);
```