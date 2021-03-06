## Front End Design questions

### I. Big questions about background
#### 1.1 full functional
#### 1.2 is it SEO or only SPA is good enough
What is SEO friendly?
-   **“SEO”**  refers to search engine optimization, or  [the process of optimizing a website](https://www.wordstream.com/blog/ws/2018/07/26/international-seo)  

Why use SEO ?
- Your content is present before you get it, so search engines are able to **index it and crawl it** just fine.
- make sure your website ranks  or indexed on Google

How to achieve SEO ?
- use Server side rendering, the content (real DOM) is much easier than virtual DOM to crawl for Googlebot.
- SSR vs. CSR (article link)
	- In **Client-side rendering,** your browser downloads a minimal HTML page. It renders the JavaScript and fills the content into it.
	- **Server-side rendering,** on the other hand, renders the React components on the server. The output is HTML content.
- how to enable SSR in React? [Next.js helps SSR](https://medium.com/swlh/server-side-rendering-with-next-js-56f84f98f9bd)

#### 1.3 PWA: yes if have time
Progressive Web Application means that it is intended to work on any platform that uses a [standards-compliant](https://en.wikipedia.org/wiki/Standards-compliant "Standards-compliant")  [browser](https://en.wikipedia.org/wiki/Web_browser "Web browser"), including both [desktop](https://en.wikipedia.org/wiki/Desktop_computer "Desktop computer") and [mobile devices](https://en.wikipedia.org/wiki/Mobile_device "Mobile device"), and for every user to use it.


#### 1.4 mobile -> mobile first, then desktop? or web is good enough, desktop first?
- eg: for B2B transactions, most people use it in larger screen, then web first is fine.

### II. MVP
List what is the MVP feature that you want to implement.

- list MVP
- list that OOS: out of scope, not covered

### III. Big picture (UI)

### IV. Data flow and User flow
- For each user-case
	for example:
	```js
	case1. user login: register notification, connection, ....
	case2. user send message
	case3. user receive a message
	```
- Define front end data structure
	for example:
	```js
	state = {
		contacts: {...},
		items: {...},
		userInfo: {...}
	}
	```

### V. state of the UI component 

### VI. how to split components and how to organize components together


### VII. challenges/bottlenecks
Eg:
- how to talk to API? [websocket vs https protocol](https://medium.com/platform-engineer/web-api-design-35df8167460)
- how to handle large traffic? - throttling 
- infinite scroll - to reduce the DOM
- consider do we need to cache some data ?
- do we need mobile?

