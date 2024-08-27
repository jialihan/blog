### 0. clarification questions

### I. Requirements (functional & non-functional)

#### 1.1 functional requirements

#### 1.2 non-functional

- all devices & mobile friendly: web is good enough, desktop first eg: for B2B transactions, most people use it in larger screen, then web first is fine.

- edge cases:

  - infinite scrolling
  - caching
  - retries

- offline chace
- Loading state/ errror /success states
- A11y
- i18n: eg: [html lang attribute](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/lang)

- SEO vs. SPA?
  What is SEO friendly?

- **“SEO”** refers to search engine optimization, or [the process of optimizing a website](https://www.wordstream.com/blog/ws/2018/07/26/international-seo)

Why use SEO ?

- Your content is present before you get it, so search engines are able to **index it and crawl it** just fine.
- make sure your website ranks or indexed on Google

How to achieve SEO ?

- use Server side rendering, the content (real DOM) is much easier than virtual DOM to crawl for Googlebot.
- SSR vs. CSR (article link)
  - In **Client-side rendering,** your browser downloads a minimal HTML page. It renders the JavaScript and fills the content into it.
  - **Server-side rendering,** on the other hand, renders the React components on the server. The output is HTML content.
- how to enable SSR in React? [Next.js helps SSR](https://medium.com/swlh/server-side-rendering-with-next-js-56f84f98f9bd)

#### 1.3 PWA: yes if have time

Progressive Web Application means that it is intended to work on any platform that uses a [standards-compliant](https://en.wikipedia.org/wiki/Standards-compliant "Standards-compliant") [browser](https://en.wikipedia.org/wiki/Web_browser "Web browser"), including both [desktop](https://en.wikipedia.org/wiki/Desktop_computer "Desktop computer") and [mobile devices](https://en.wikipedia.org/wiki/Mobile_device "Mobile device"), and for every user to use it.

### II. Component Design (Big picture/Architecture/UI)

- For each user-case
  for example:
  ```js
  case1. user login: register notification, connection, ....
  case2. user send message
  case3. user receive a message
  ```
- Drawing pictures of client <---> server

### III. Data Model

3.1 componentProps:

```
type componentArgs = {
	// data
	resultData: [],
	minSearchLen: number,
	// cache
	// interactions/callback
	onChange: ()=>{},
	onFocus:()=>{},
	onBlur: ()=>{},
	onSubmit: ()=>{},
	// styling
	style: string
	className: string,
	// debounce
	debounceTime: number,
}
```

3.2 Define front end data structure
for example:

```js
state = {
	contacts: {...},
	items: {...},
	userInfo: {...}
}
```

### IV. Interface (API)

- HTTP1.0 vs. HTTP 2.O: half-duplex
- GraphQL: Modern API, http2 compatibility, `pull the data you need`.
- websockets: full-duplex communication over a single TCP, speed is fast. Cons: hard to load balance (eg: consistent hashing), if one server is down, ....

### V.Optimization/improvment

#### 5.1 Network Performance

- compress headers, GZIP files(js file, css files)
- http caching
- Batch Requests
- impage optimization: pull compressed images by fixed size from [microservices](https://www.atlassian.com/microservices/microservices-architecture/microservices-vs-monolith)("a series of independently deployable services.")
- code-splitting: lazy load, vendor bundle (eg: tracking, analysis, etc..)

#### 5.2 Rendering Performance

- mobile fridenly and responsive UI
- client side cache: local storagem cookies, [session storage](https://developer.mozilla.org/en-US/docs/Web/API/Window/sessionStorage)
- `preload` JS when needed
- SSR -> next.js
- error states / success states

#### 5.3 A11y

- use `aria-label` on non-text content
- keyboard navigation
- semantic html
- device tests

#### 5.4 Security

- XSS -> sanitize HTML input before sending to API
- CORS

### Appendix

<s> #### [Outdated]VII. challenges/bottlenecks</s>

Basically consider it with two areas:

- Smoothness
- Speed

#### 7.1 Smoothness

- **Instance go back** ( Page Stack, global state, api caching)
  Problem: we wanna recover to UI state, we wanna previous data
  - global state/cache data: store data in memory instead of request API again
  - api caching: [Web API - cache](https://developer.mozilla.org/en-US/docs/Web/API/Cache)
    - The **`Cache`** interface provides a persistent storage mechanism for [Request](https://developer.mozilla.org/en-US/docs/Web/API/Request) / [Response](https://developer.mozilla.org/en-US/docs/Web/API/Response) object pairs that are cached in long lived memory.
    - Note that the `Cache` interface is exposed to **windowed scopes** as well as workers.
  - **Page Stack**: don't destroy the recent screen in DOM, might + `sliding window`, at least caching two screens for current screen, and previous screen when going back. Docs: [@react-navigation/stack](https://reactnavigation.org/docs/stack-navigator/)
- **Instant go forward** (Skeleton, loading indicator, above-the-fold)
  - **Skeleton**: feeds the user with an instant new ui, which is a skeleton screen with some color
  - **loading indicator**
  - **above-the-fold:** cut the ui into half, render the up the fold first, after it becomes active, then render the below the fold right part.
- **Instant interaction response**(A11y, passive listener, design guidelines)
  - passive listener to improve scrolling performace: [stackoverflow](https://stackoverflow.com/questions/37721782/what-are-passive-event-listeners), [mdn-event-listener](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener#improving_scrolling_performance_with_passive_listeners)
    - problem: current browser need to wait for the results of any `touchstart` and `touchmove` handlers, which may prevent the scroll entirely by calling `preventDefault()` on the event.
    - Solution: `{passive: true}`, by marking a touch or wheel listener as passive, the developer is promising the handler won't call `preventDefault` to disable scrolling.
  - design guidelines: eg: button size, error pop up, transition and animation for pop ups.
- **Jank-free Animations:** support for lower-end devices, eg: [lottie animations](https://lottiefiles.com/), do it progressively ( for images, or animations), or remove the animations if it's a blocker on UI.
- **make the app more native-like:**
  - native-like animation/transitions/**gestures**, eg: sliding, pop up transitions
  - native-like UI component: eg: native like [action sheet](https://developer.apple.com/design/human-interface-guidelines/ios/views/action-sheets/#:~:text=An%20action%20sheet%20is%20a,at%20once%20as%20a%20popover.) menu, toast (for network done, errors).

#### 7.2 Speed

- **resource preload / prefetch**: [reference](https://css-tricks.com/prefetching-preloading-prebrowsing/)
  ask the browser to request that item and store it in the cache for reference later. Unlike prefetching assets, which can be ignored, preloading assets **must** be requested by the browser.
  ```html
  <link rel="prefetch" href="image.png" />
  <link rel="preload" href="image.png" />
  ```
- **Code splitting/Skeleton:** [reference](https://developer.mozilla.org/en-US/docs/Glossary/Code_splitting)
  Code splitting is the splitting of code into various bundles or components which can then be loaded on demand or in parallel.
  - webpack manages the bundles
  - App bundle -> lazy load
  - Vendor bundle -> tracking, 3rd party libraries
  - ES2020 - dynamic import
    ````js
    import('/modules/some-module.js')
      .then((module) => {
        // Now you can use the module here.
      });
    // or
    let module = await import('/modules/some-module.js');	```
    ````
  - [React - code splitting](https://reactjs.org/docs/code-splitting.html): multiple ways
  - Skeleton:([article1](https://blog.logrocket.com/improve-ux-in-react-apps-by-showing-skeleton-ui/), [article2](https://css-tricks.com/building-skeleton-components-with-react/)):
    - bundle all skeleton (but it's still have some cost).
    - OR just apply to important components/screens/flows, eg: Timeline page, Detail page, ... are important + others with loading indicator
    - use `prefetch`: which screen user will go next
    - **Question**: how to implement a skeleton?
      - create separate version of components
      - OR: one component with dummy data, split within
      - React 3rd party libraries: [react-loading-skeleton](https://github.com/dvtng/react-loading-skeleton), [react-placeholder](https://github.com/buildo/react-placeholder)
- Caching / CDN
  - static resources - cache
    For any subsequent request for that resource, the browser uses its local copy, rather than going to the network to fetch it.
  - CDN provides for different people in this world to have a faster access to these resources.
- cache API with [Service worker](https://developers.google.com/web/ilt/pwa/caching-files-with-service-worker#:~:text=The%20Service%20Worker%20API%20comes,The%20entry%20point%20is%20caches%20.) / offline
- **Lazy-load**: eg: an image, a component, everything....
- **Auto pager:** for news feed to fetch timeline things, page can scroll and automatically append new content.
- **infinite scroll**
- **SSR/ initial data feed**
  - SSR: cost much more work, use SSR framework: [`Next.js`](https://nextjs.org/docs/getting-started)
  - initial data feed: barebone HTML + api fetch later
- **Within viewport update:** eg: stock update API, chat, like/unlike counts update, just update elements in the viewport.

#### 7.3 About images

- Compress: use mircrosevices to pull the compressed image by fixed size
- Lazy load / place holder
- [Progressive images:](https://www.keycdn.com/support/progressive-images#:~:text=A%20progressive%20image%20is%20interlaced,with%20each%20additional%20%22pass%22.&text=Progressive%20images%20give%20the%20end,before%20it%20is%20completely%20downloaded.) A **progressive image** is interlaced meaning the **image** will start out as low quality, before it's completely downloaded.
- use SVG for icons
- Caching / http2
  - browser cache, cdn cache the images
  - for SVG - inline it
    inline SVGs don’t cause an additional request, since they are a part of the HTML document.
  - for other images, should support http2: TODO

#### 7.4 About APIs

- Poll / Websocket (eg: chat app) / SSE
  - update/poll new date when needed
  - websocket: send & receive things, eg: chat app
  - Server send event: [docs](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events/Using_server-sent_events), when client want to listen to some event
- GraphQL: [pros & cons article](https://www.altexsoft.com/blog/engineering/graphql-core-features-architecture-pros-and-cons/)
- BFF: backend for frontend (api aggregating)
- Caching / http2: cache apis

#### 7.5 RAIL performance model

**Docs:** [reference1](https://web.dev/rail/), [reference2](https://www.keycdn.com/blog/rail-performance-model#:~:text=The%20RAIL%20model%20takes%20into,interactions%20under%20these%20four%20domains.)

- response (100ms)
- animation (frame within 10ms)
- idle (use idle time 50ms)
- load ( 5 seconds)

#### 7.6 Matrix for page to load

1. DOMContentLoaded
2. load
3. First Contentful Paint - **FCP**
4. first meaning paint(deprecated)
5. [Speed index](https://web.dev/speed-index/)
6. first CPU Idle - ready to interact, deprecated
7. Time To fully Interactive - **TTI**
8. first input delay - user wait response time
9. total blocking time - FCP to TTI (3rd to 7th)
10. Largest Contentful paint

Eg:

- how to talk to API? [websocket vs https protocol](https://medium.com/platform-engineer/web-api-design-35df8167460)
- how to handle large traffic? - throttling
- infinite scroll - to reduce the DOM
- consider do we need to cache some data ?
- do we need mobile?

### VIII. Review steps

#### 8.1. understand what it is

#### 8.2 decide the scope that suits (Goal)

Note: also list up TODO and not TODO (out of scope)

1.  What is the basic goal of the feature (functional goal)  
     eg: user authentication, user search people, user search jobs, user chat.......
2.  what is the NON-Functional goal?
    They specify criteria that judge the operation of a system, rather than specific behaviours, for example: “Modified data in a database should be updated for all users accessing it within 2 seconds.”
    **Some typical non-functional requirements are:**

    - **Performance** – for example Response Time, Throughput, Utilization, Static Volumetric
    - **Scalability** - [article](https://www.bornfight.com/blog/10-principles-of-scalable-frontend-projects/):
      - re-usable components, not screens;
      - separate pure UI components & buisiness logic,
      - data-fetching (maybe improve use graphql?),
      - use UI libraries or build the **consistent UI standards**
      - use **storybook** to create resusable components as a style guide for your app
      - use linters & formaters......
    - Capacity - what is the DAU, TPS.... load speed on website when user >10000, how many user simultaneously enter our website?
    - **Availability**: An available website is a website that is accessible and usable as expected by the user. How to test? ping a url, test.... - [article1](https://www.uptrends.com/what-is/website-availability), [article2](https://docs.microsoft.com/en-us/azure/azure-monitor/app/monitor-web-app-availability)
    - **Security**: XSS attacks & Cross-Site-Request-Forgery (CSRF), [docs_MDN](https://developer.mozilla.org/en-US/docs/Learn/Server-side/First_steps/Website_security)
      Protocal: [HTTPS](https://en.wikipedia.org/wiki/HTTPS) (TSL, SSL secure socket layer), SFTP (for file transfer, known as FTP over SSL/TLS)
      HOW to prevent?
      - server layer
      - network layer
      - web app:
        - XSS: validate input, sanitizing().....
        - [CSRF](https://www.netsparker.com/blog/web-security/csrf-cross-site-request-forgery/): 1) anti-CSRF token, 2) use the [SameSite Flag](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie/SameSite) in cookie, which only allows the request that originate from the same domain.
    - **Usability**: a [website](https://en.wikipedia.org/wiki/Website)[[1]](https://en.wikipedia.org/wiki/Web_usability#cite_note-1) are broad goals of [usability](https://en.wikipedia.org/wiki/Usability) and presentation of [information](https://en.wikipedia.org/wiki/Information) and choices in a clear and concise way: how to improve ?
      - [Optimize for mobile devices](https://www.quicksprout.com/website-usability/#optimize)
      - [Follow WCAG standards](https://www.quicksprout.com/website-usability/#follow): web content accessiblity guideline
      - [Stick to common design elements](https://www.quicksprout.com/website-usability/#stick)
      - [Simplify the navigation](https://www.quicksprout.com/website-usability/#simplify)
      - [Establish credibility](https://www.quicksprout.com/website-usability/#establish)
      - [Make sure your content is legible](https://www.quicksprout.com/website-usability/#make)
      - [Be consistent](https://www.quicksprout.com/website-usability/#be)
    - **Reliability**: [how to check a website reliability?](https://sites.google.com/a/online.island.edu.hk/island-school-research-centre/assessing-the-reliability-of-websties)
      - purpose
      - accountable - check url: eg: `.com, .gov, .lib,....`
      - current - recently updated
      - content
      - accurate
    - Data Integrity: keeping your data consistent at all times, how to protect?
      - validate at the source
      - end-to-end data lineage
      - set access privilege and controls
      - comply with the regulation: eg: # General Data Protection Regulation(**GDPR**) ....
    - Recoverability: build disaster recovery plans, ensure global traffic balance on server side, backup your website data...

3.  What is your non-functional goal of the feature
4.  What is the data flow(api) /user flow - with expanation
5.  What is the MVP
6.  What is the state of the UI component 7. How would you separate the min parts and put then together (UI/logic) 8. What is the core spec

**TODO(OOS):**

- cross team communication, each team will focus architecture their part (and each flow can separately deployed), share components, share utils
- if more time: separate HTML, multiple SPA
- design system: consistent design style (main for design teams, also for front-end devs to join)

#### 8.3 Assumptions on background

- Suppose the [DAU/MAU](https://www.klipfolio.com/metrics/saas/dau-mau-ratio) of the service
- Suppose how many interactions occurs in a day
- Suppose 300KB is tolerable
- Suppose average api response is 100ms
- ... .etc

#### 8.4 Big Picture

- Draw a diagram or list up the outline
- Data flow / User interaction flow
- Check with interviewer

#### 8.5. Key challenges, bottleneck

- smoothness
- speed

#### 8.6. Trade-off, alternatives, TODO

Nothing is perfect.  
Try to list up possible improvement ideas, And things you wanana do if more time given.

**For exmaple:**

- try graphql on a part of the app, then migrate the whole app to the application
- more fancy features if more time
