## Cookie and Session Management

### 1. what are cookies?

Cookies are produced and shared between the browser and the server using the HTTP Header.

[!image](../assets/cookie_01.png)

- the browser will **always** send COOKIES with every request
- the server will send back the cookie with "**sessionID**"

### 2. Session

Usually managed on server side.
A session ends when the user closes the browser or after leaving the site, the server will terminate the session after a predetermined period of time, commonly 30 minutes duration.

[!image](../assets/cookie_02.png)

### 3. HTTP is a stateless protool

So that cookie makes it more stateful between client and server.

### 4. Additional Resources

- MDN docs: [Using HTTP cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies)
