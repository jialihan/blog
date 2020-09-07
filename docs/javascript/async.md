## Async Javascript

### I. Background
* Javascript is a programming language, it's **single-threaded**.
* browser or nodejs allow us to use **async code**
* Async Function is function that we can execute later:
	For example: **calling webAPIs**
	- setTimeout( ()=> { // function later to execute }, 1000);
	- Promise.resolve('hi').then(()=>{ // function later to execute});
	- DOM
	- AJAX (XMLHttpRequest)
* WebAPI flow
	1) web api call
	2) running outside javascript engine
	3)  after finished: put into call back queue
	4) event loop checks available execution stack
	5)  push it back to our execution stack

### II. Async vs. Sync in Javascript
#### 1. Javascript engine
For example: google chrome -> V8 engine
 * **memory heap**: allocate memory
	For example: 
	```
	const x = 1; // global variable
	```
	Problem: **memory leak:** 
	 memory leaks happen when you have unused memory such as not using the variable. Global variable might be bad.
* **execution stack**: parse and execute scripts  
    - do one thing only: **single threaded**
    - first in last out
 Problem: stack overflow
      For example:
      ```
      function foo()
      {
	      foo();
      }
      foo(); // stack overflow
      ```
      
	| foo() | overflowed |
	|--|--|
	| foo()| 
	| foo()| 
	| ... | 
	| foo()| 
	| foo()| 
#### 2. Javascript Run-Time environment
* engine
* web APIs
* callback queue: onClick, onLoad, onDone
* event loop: `element.addEventListener('click', funcHandler);`

#### 3. Promises
A a promise is an object that may produce a single value in the future, promise has three possible states: `fulfilled, rejected or pending`.

1 ) CallBack
execute this piece of code when something is done.
eg: 
```
el.addEventListener("click", submitForm);
```

2 ) Create a Promise

3) Catch error

4) `Promise.all( [promise1, promise2, promise3......] )`
return array of values that each promise produced.

#### 4. Aysnc Await ( ES8)
The goal with async await is to make **asynchronous code to look  like synchronous code**.



####
1. threads
2. concurrency and parallelism

* Concurrency(single-Core CPU):

|  |  |
|--|--|
|  th1 |  |
| th1  |   |
|   | th2  |
|  th1 |  |
|   | th2  |

* Concurrency + parallelism (Multi-core CPU)

|  |  |
|--|--|
|  th1 | th2 |
|   | th2   |
|   | th2  |
|  th1 |  |
|   | th2  |
