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
```
const promise =  new Promise((resolve,  reject)  =>  {
if (true) {
	resolve('success');
	}  else  {
	reject('error');
	}
});
```
2.1 ) How to run the promise?
use `.then()`
```
promise
.then((result)  => result +  '!')
.then((result2)  => result2 +  '?')
```

3 ) Catch error: use `.catch()` method
It only catch between code, if error is after catch() method, it won't get caught.
```
promise
.then((result)  => result +  '!')
.catch(()  =>  console.log('error'))
.then((result3)  =>  {
	throw  Error;
	console.log(result3);
});  // won't catch this error
```
 
4 ) `Promise.all( [promise1, promise2, promise3......] )`
return array of values that each promise produced, these promises are running **in parallel**. 

#### 4. Aysnc Await ( ES8)
The goal with async await is to make **asynchronous code to look  like synchronous code**.
* **async** keyword in function
* `try{} catch{}` block
* **await** keyword

Code example:
```
async  function()  {
try  {
	const  [ users, posts, albums ]  =  await  Promise.all(
		urls.map((url)  =>  {
			return  fetch(url).then((resp)  => resp.json());
	}));
	console.log('success');
}  catch (err) {
	console.log('error:', err);
}
};
```
#### 5.  new features in ES9: Aysnc Await 
1 ) `finally{}` block
it will be worked no matter then or error caught, it will be executed.
```
myPromise
.then(resp=>resp.json())
.catch()
.finally(()=>{
	console.log('finally done');
});
```
 2 ) await in **for loop**
it's used to process an array of promises in the same logic pattern:
it iterates the async objects: [Reference/Statements/for-await...of](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for-await...of)
```
try  {
	const arrayOfPromises = urls.map((url)  =>  fetch(url));
	for  await (let request of arrayOfPromises) {	
		const data =  await request.json();
		console.log('data', data);
	}
}  catch (err) {
} 
```
#### 6. callback queue vs job queue
* callback -> web APIs -> external call: use **callback queue**
* Promise -> inside Javascript engine -> use **job queue**
* Priority order:  **job queue > callback queue**

#### 7. parallel, sequence, race
* promises.**all()** : run in parallel on the promise array
* promises.**race()**: return the **first** result that promise finished
* sequence:
	 ```
	  await  a();  // pause
	  await  b();  // pause
	  await  c();  // pause
	 ```


#### 8. thread and concurrency
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





