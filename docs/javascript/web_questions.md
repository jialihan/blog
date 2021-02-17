##  Most Common web interview questions & answer
   
#### I. [Behind Screens: What happens when you type a URL in the web browser?](#question-1)  

#### II. [Reasons for your page is very slow & Solution](#question-2)

<div id="question-1" />

### I. Potential Security Holes

#### 1.1 Reference articles: [article 1](https://wsvincent.com/what-happens-when-url/), [article 2](https://afteracademy.com/blog/what-happens-when-you-type-a-url-in-the-web-browser)

#### 1.2 Steps
- 1 ) You enter a URL into a web browser
- 2 ) The browser looks up the IP address for the domain name via DNS (domain name server)
- 3 ) The Browser initiates a TCP connection with the server
	-   **Step 1 (SYN):** As the client wants to establish a connection so it sends an SYN(Synchronize Sequence Number) to server
	-  **Step 2 (SYN + ACK):** If the server is ready to accept connections and has open ports then it acknowledges the packet sent by the server with the SYN-ACK packet.
	- **Step 3 (ACK):**  In the last step, the client acknowledges the response of the server by sending an ACK packet. 
- 4 ) The browser sends a HTTP  _request_  to the server
- 5 ) The server sends back a HTTP  _response_
- 6 ) The browser begins rendering the HTML (barebone) from server response
- 7 ) The browser sends requests for additional objects embedded in HTML (`images, css, JavaScript`) and repeats steps 4-6.
- 8 ) Once the page is loaded, the browser sends further **async requests** as needed.


<div id="question-2" />

### II. Reasons for your page is very slow & Solution

#### 1. server side consideration
- 1.1 server performance
- 1.2 server location: slow web hosting
	- Solution:  use [Content Delivery Network (CDN)](https://www.dreamhost.com/academy/what-is-cdn/) consists of several servers that are placed in strategic geographic locations. You can store copies of your website on them so its pages can be quickly loaded by users who are **located far away from your main server.**
- 1.3 lots of traffic


#### 2. assets
- 2.1 Large images & video files are increasing loading times
	- solution: compression & smush techniques
- 2.2 Your Site’s CSS Isn’t Optimized
	Solutions:
	-   If you have several external CSS files, combine them into one or a few files.
	-   Remove external CSS and [use inline CSS](https://developers.google.com/speed/docs/insights/OptimizeCSSDelivery) instead.
	-   [Use “media types”](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/render-blocking-css) to specify when certain CSS files should be loaded.

#### 3. code density
- 3.1 Your Site’s Code Is Too Bulky
The more code your user’s web browser has to load, the longer it will take for your website to become visible. If your code is too “bulky” or contains unnecessary characters and line breaks, your site may be slower.
	- Solution:  you can “minify” that code by removing the elements that aren’t needed.
	- Solution2: better website design, not so tight & densive elements in layout to render.

#### 4. browser cache
- 4.1 Caching Issues Are Preventing Optimized Page Loading
	Caching is when browsers store static copies of your website’s files. Then when users access your site, their browsers can display the cached data instead of having to reload it.
	- Solution: some online cache strategy