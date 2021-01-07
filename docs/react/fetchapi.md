## Using fetch API - Disadvantages  

#### I. [Why not use fetch() ?](#question-1)

#### II. [HTTP Status Code Summary](#question-2)

#### III. [Handle Http Errors in Fetch Api](#question-3)

#### IV. [Use await in Async function - fetch() ](#question-4)


<div id="question-1"/>

### I. Why not use fetch() api ?

- need extra code to parse json data
- bad at error handling
	- The default `.catch()` block won’t get http status code errors.
	- Basically `fetch()` will only reject a promise if the **user is offline (`failed request`)**, or some unlikely **networking error** occurs, such a **DNS lookup failure**.

<div id="question-2"/>

### II. HTTP Status Code Summary

1xx：Informational 临时响应信息  
2xx：Successful 成功
3xx：Redirection 重定向
4xx：Client Error 客户端错误  
5xx：Server Error 服务器错

Note:
- fetch api **won’t reject on HTTP error status** from `400 <= code < 600`. Instead, it will resolve normally (with `ok` status set to false).
- example fetch api response when http error happens:

	 ![image](../assets/fetchhttperror.png ':size=670x104')
	 
	 response object: `ok = false`




<div id="question-3"/>

### III. Handle Http Errors in Fetch Api

Code Example:
- parse json 
- add extra code to reject HTTP status error
	- reject by response.ok (false)
	- reject by custom status code 400 <= code < 600, even you can parse the exact code value.

**Example 1:** reject by `response.ok`
```js
fetch('some-url')
  .then(response  => {  
	  if(!response.ok) {  
		// handle error
		return Promise.reject('something went wrong!');
	  } 
	  return response.json()  
  })
  .then(data  =>  console.log(data))  
  .catch(error  => console.log(error));
```

**Example 2:** reject by specific `error status code`
```js
fetch('some-url')
  .then(response  => {  
      if (response.status >= 400 && response.status < 600) {
	      // handle error
          throw new Error("something went wrong!");
    }
      return response.json();
  })
  .then(data  =>  console.log(data))  
```


<div id="question-4"/>

### IV. Use await in Async function - fetch()

**Docs:** [await keyword](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Async_await#the_await_keyword)

**Syntax:**
```js
async function boo()
{
	try {
		// async function using await keyword
		const resp = await Promise.reject("wrong");
	}
	catch(err)
	{
		// handle error
	}
}
```

**Rewrite** fetch() using `await`:
```js
async function myFetch() {
	try {
		let response = await fetch('some-url');
		if (!response.ok) {
		    throw new Error(`HTTP error! status: ${response.status}`);
		} 
		response = await response.json();
		
		// process data
		console.log(response);
		
		return response;
	}
	catch(err)
	{
		console.log(err);
	}
}
```



