## Section 12 Routing and Router in VUE

### 12.1 introduction to vue Router

DOC: [Vue Router](https://router.vuejs.org/installation.html)

Background:
in old days, the routing is done on BE for some languages, requesting an entire page, eg: Java, PHP, Python.

```
client side "/"  ------Request home page------> Server Router
                 <-----Respond page content----
```

But in modern Javascript, browser has the ability to handle the routing on client side. The component only require the `pure data` to the backend, then only **partial of the page is re-rendered**.

```
client side "/about" --request about page--> Component --request data--> Server
                 <--respond component/data--          <---respond data---

```

How to install routing packages in VUE? - most popular solution

```
npm install vue-router
```

#### 12.2 configure the router

At first, when config the `vue instance` in `main.js` file:

```js
app.use(router);
```

under the folder path: `/src/router/index.js`:

```js
import { createRouter, createWebHashHistory } from "vue-router";

const router = createRouter({
  history: createWebHashHistory(),
  routes: [
    //...
  ],
});
```

Explain these helper functions:

- `createRouter({})`
- `createWebHistory`: [doc](https://router.vuejs.org/api/#createWebHistory) - help to interact with browser's history (history api) ``

  - `import.meta.env.BASE_URL(string)`: vite built-in **env variables** ([doc](https://vitejs.dev/guide/env-and-mode.html#env-variables)), eg: Full URL, e.g. `https://foo.com/`(the origin part won't be used in development).

#### 12.3 creating Routes

**Tip**💡:
better Practice: `outsource to an variable` for all routes in this project:

```
const routes = [];
```

#### 12.3.1 Props of an route object

Props:

- `path`: string
- `component`: vue-component object, `/components` folder to store generic components, while the `/views` folder to store components that will act `as container for the pages content`.

Example of one `route object`:

```js
[
  {
    path: "/",
    name: "home",
    component: Home,
  },
];
```

#### 12.3.2 add Router into template

`router-view`([doc](https://router.vuejs.org/guide/#router-view)) will display the component that corresponds to the URL. You can put it anywhere to adapt it to your layout.

```vue
<router-view></router-view>
```

For example:

![image](./docs/VUE/section12-3-2_01.png)

### 12.4 different history modes

The `{history: ...}` option when creating the router instance allows us to choose among different history modes.

- `Hash Mode`:[doc](https://router.vuejs.org/guide/essentials/history-mode#Different-History-modes)
- `HTML5 Mode`: [doc](https://router.vuejs.org/guide/essentials/history-mode#HTML5-Mode)
- etc.: `Memory mode`,...

#### 12.4.1 Hash Mode

It uses a hash character (`#`) before the actual URL that is internally passed. Because this section of the URL is never sent to the server, it doesn't require any special treatment on the server level.
Exampple:

```
https://jialihan.github.io/blog/#/react
https://jialihan.github.io/blog/#/VUE/intro_01
<a href="#/VUE/intro_01">
```

**API:**
`createWebHashHistory`: https://router.vuejs.org/api/#createWebHashHistory.

**Benefit:**

- page didn't refresh, no neeed to download assets again
- legacy/old browsers also support this hash system
- when configuring a server to handle any URL is not possible.

**Cons:**

- ** It does however have a bad impact in SEO.** If that's a concern for you, use the `HTML5 history mode`.
- users usually doesn't type `#` on the URL, not convenient.

#### 12.4.2 HTML5 mode - [doc](https://router.vuejs.org/guide/essentials/history-mode#HTML5-Mode)

**Important:**
There is a `refresh` when changing route, and in network tab the browser will download JS files, assets again.

**Solution:**
When using `#` hash is because browser smartly know where is the content after a `#` hash relates to a content on the page.
And we can add a `Link` to implement the Hash system, for example: the `navigation bar links`.

#### 12.4.3 how page deliver to the client

- 1. Server always has priority for handling request
- 2. Vue (as a client library) won't run until the application delivered to the client.
- 3. server should serve an **indexed HTML file** regardless of the URL.

  - 3.1) if using Vite, by default it's serving the `index.html`.
  - 3.2) if using other server, you need to **configure the server** to serve the `index.hmtl`: [doc](https://router.vuejs.org/guide/essentials/history-mode#Example-Server-Configurations)

    ```js
    // example node.js
    const http = require("http");
    const fs = require("fs");
    const httpPort = 80;

    http
      .createServer((req, res) => {
        fs.readFile("index.html", "utf-8", (err, content) => {
          if (err) {
            console.log('We cannot open "index.html" file.');
          }

          res.writeHead(200, {
            "Content-Type": "text/html; charset=utf-8",
          });

          res.end(content);
        });
      })
      .listen(httpPort, () => {
        console.log("Server listening on: http://localhost:%s", httpPort);
      });
    ```

  - 4. Then Vue will handle the routing and components

### 12.5 Navigating with Links

`<router-link>` ([doc](https://router.vuejs.org/guide/#router-link))component is similar to a `<a>`anchor tag element:

- `<a>` to `<router-link>`
- `href="/about"` to `to="/about"`

> **Note**: whether to add the `/`(forward slash) on your path, yes, we need to add if you want your path is based on the `base domain`. Otherwise, it will append based on `current path`.

Issue example:

```
http://localhost:5173/foo/boo
// ❌ without '/', it overrides current path
http://localhost:5173/foo/about
// ✅ with '/'
http://localhost:5173/about
```

### 12.6 Tailwind Styles for Active Links

12.6.1
Background: the `<router-link>` will provide some default styles for the `:active` state:

```html
<a
  href="/about"
  class="router-link-active router-link-exact-active
  text-white px2"
  aria-current="page"
></a>
```

But we can **customize** it: ([vue-doc](https://v3.router.vuejs.org/api/#linkexactactiveclass))

- override the class name
- config css class names in the `router/index.js config file`: partial match or exact match

```js
const router = createRouter({
  // ...
  linkExactActiveClass: "text-yellow-500",
});
```

12.6.2 override the css class on a specific link:
`exact-active-class`: [doc](https://v3.router.vuejs.org/api/#exact-active-class)

- type: string
- default: `"router-link-exact-active"`
- Configure the active CSS class applied when the link is active with **exact match**.

Example code:

```html
<router-link
  class="text-white text-2xl mr-4"
  to="/"
  exact-active-class="no-active"
></router-link>
```

### 12.7 Named Routes - add a name to the route

A route with a name can be referenced by its name rather than its path.
In `/router/index.js` to add a `name` property:

```js
const routes = [
  {
    path: "/",
    name: "home",
    component: Home,
  },
];
```

To use the `name` of the route instead of `path` of the route in the web application, on the `:to` property:

```
// old
 to="/"
// new: bind the attribute
:to="{ name: 'home' }"
```

### 12.8 non-exist route and redirection route

#### 12.8.1 non-exist route

shows an empty page on the browser: eg: `/unknown`, options to handle this case:

- 1. render a 404 message: not found
- 2. redirect user to the `new path`(the correct page)

Use the `redirect property` in router config file, [doc](https://router.vuejs.org/guide/essentials/redirect-and-alias#Redirect)

```js
// /router/index.js
  {
    path: "/manage",
    redirect: "/manage-music",
  },
```

#### 12.8.2 "catch-all" route/ 404 not found route

use `:catchAll` or `:pathMath`, doc: https://router.vuejs.org/guide/essentials/dynamic-matching#Catch-all-404-Not-found-Route

```js
[
  {
    // path: "/:catchAll(.*)*",
    path: "/:pathMatch(.*)*",
    redirect: { name: "home" },
  },
];
```

### 12.9 Route Alias

doc: https://router.vuejs.org/guide/essentials/redirect-and-alias#Alias

`alias prop` is an additional path: the value is a path string,
eg:

```js
{
  path: "/manage-music",
  component: Manage,
  name: "manage",
  alias: "/manage",
},
```

Note: comparing with redirect route, when to use?

> Note about SEO: when using aliases, make sure to define [canonical links](https://developers.google.com/search/docs/crawling-indexing/consolidate-duplicate-urls?hl=en&visit_id=638376861835662375-2311211209&rd=1).
> Reason:
> To specify which URL that you want people to see in search results. You might prefer people to reach your green dresses product page via https://www.example.com/dresses/green/greendress.html rather than https://example.com/dresses/cocktail?gclid=ABCD.

### 12.10 Guarding Routes

doc:[Navigation Gaurds](https://router.vuejs.org/guide/advanced/navigation-guards.html)
Guards are defined as `functions`, we can **run a function before user navigates to a specific page**, like a hook.

#### 12.10.1 option1: global guard

```js
router.beforeEach((to, from, next) => {
  console.log("global guard: ", to, from);

  // the component won't load until the next() func is called.
  next();
});
```

#### 12.10.2 specific route guard

[beforeEnter()](https://router.vuejs.org/guide/advanced/navigation-guards.html#Per-Route-Guard): guards only trigger when entering the route, they don't trigger when the params, query or hash change e.g. going from `/users/2` to `/users/3` or going from `/users/2#info` to `/users/2#projects`. They are **only triggered when navigating from a different route**.

```js
beforeEnter: (to, from, next) => {
  // ...
};
```

#### 12.10.3 define a guard in a component

```vue
<script>
export default {
  name: "mamage", // comp name, better to have
  beforeRouteEnter(to, from, next) {
    console.log("beforeRouteEnter guard");
    next();
  },
};
</script>
```

Note: the order of the priority of the guards, please see the below console message:

![image](./docs/VUE/section12-10_01.png)

**The Full Navigation Resolution Flow:** ([doc](https://router.vuejs.org/guide/advanced/navigation-guards.html#The-Full-Navigation-Resolution-Flow)):

- Navigation triggered.
- Call `beforeRouteLeave` guards in deactivated components.
- Call **global** `beforeEach` guards.
- Call `beforeRouteUpdate` guards in reused components.
- Call `beforeEnter` in **route configs**.
- Resolve async route components.
- Call `beforeRouteEnter` in **activated components**.
- Call global `beforeResolve` guards.
- Navigation is confirmed.
- Call global `afterEach` hooks.
- DOM updates triggered.
- Call callbacks passed to next in `beforeRouteEnter` guards with instantiated instances.

### 12.11 Restrictions on Route

If the user is logged in, we allow to enter the route. use the `UserStore` in the guard functions to implement this.
`next()`: 3rd parameter, [doc](https://router.vuejs.org/guide/advanced/navigation-guards.html#Optional-third-argument-next.)

```js
// check user logged in or not
const store = useUserStore();
if (store.isLoggedIn) {
  next();
} else {
  // redirect to home page
  next({ name: "home" });
}
```

### 12.12 Redirect after Logging Out

```js
signOut() {
  this.userStore.signOut();
  if (this.$route.name === "manage") {
    // access router object in components, vue will inject it.
    this.$router.push({ name: "home" });
  }
},
```

### 12.13 Route Meta Fields

doc:https://router.vuejs.org/guide/advanced/meta.html

Problem: manually restrict multiple routes to enter

```js
if (
  this.$route.name === "manage" ||
  this.$route.name === "page1" ||
  this.$route.name === "page2" ||
  this.$route.name === "page3" ||
  this.$route.name === "page4"
) {
  // redirect to home page
}
```

**Solution:**
define a meta field on the `router.js config`:

```js
{
  path: "/manage-music",
  component: Manage,
  name: "manage",
  beforeEnter: (to, from, next) => {
    next();
  },
  meta: {
    requiresAuth: true,
  },
},
```

Then the app will check the `meta[fieldKey]` instead of the route name `this.$route.name`:

```js
if (this.$route.meta.requiresAuth === true) {
}
```

**Improved Solution**: check meta field on global **level**

```js
router.beforeEach((to, from, next) => {
  console.log("to.meta field", to.meta);
  if (!to.meta.requiresAuth) {
    // no meta field
    next();
  } else {
    // check useStore.isLoggedIn
    // ...
  }
});
```
