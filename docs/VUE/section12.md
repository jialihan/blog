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

### 12.6 Custom Links
