## Onboard to Next.JS - 01

### 1. What is Next.js?

Next.js is a React framework for building `full-stack` web applications. You use React Components to build user interfaces, and Next.js for **additional features and optimizations**.

Under the hood, Next.js also abstracts and automatically **configures tooling** needed for React, like bundling, compiling, and more.

### 2. Main Features - [doc](https://nextjs.org/docs#main-features)

- [Routing](https://nextjs.org/docs/app/building-your-application/routing)
- [Rendering](https://nextjs.org/docs/app/building-your-application/rendering)
  Client-side and Server-side Rendering with Client and Server
  Components. Further optimized with Static and Dynamic Rendering on
  the server with Next.js.[Streaming on Edge and Node.js runtimes](https://vercel.com/blog/streaming-for-serverless-node-js-and-edge-runtimes-with-vercel-functions). Note: [HTTP Live Streaming](https://caniuse.com/http-live-streaming) not widely used in web browsers.
- [Data Fetching](https://nextjs.org/docs/app/building-your-application/data-fetching)
  Simplified data fetching with async/await in Server Components, and
  an extended `fetch` API for request memoization, data caching and
  revalidation.
- [Styling](https://nextjs.org/docs/app/building-your-application/styling)
  Support for your preferred styling methods, including CSS Modules,
  Tailwind CSS, and CSS-in-JS
- [Optimizations](https://nextjs.org/docs/app/building-your-application/optimizing)
  Image, Fonts, and Script Optimizations to improve your application's
  Core Web Vitals and User Experience.
- [TypeScript](https://nextjs.org/docs/app/building-your-application/configuring/typescript)

3. [Installation guide]
   install doc: https://nextjs.org/docs/getting-started/installation

4. [App router vs. Page router](https://nextjs.org/docs#app-router-vs-pages-router)

- App Router is a newer router that allows you to use React's latest features, such as `Server Components and Streaming`.
- The Pages Router is the original Next.js router, which allowed you to build server-rendered React applications and continues to be **supported for older Next.js** applications.

5. Learn to build the [dashboard application](https://nextjs.org/learn/dashboard-app)

- [pNPM](https://pnpm.io/) -> faster then yarn and npm
- `create-next-app` command
- config file in nodeJS: `next.config.mjs`, Node.js will treat `.cjs` files as CommonJS modules and `.mjs` files as ECMAScript modules. It will treat `.js` files as whatever the default module system for the project is (which is **CommonJS** unless `package.json` says `"type": "module"`,).
- most files have a `.ts` or `.tsx` suffix.
- `pnpm i` : install
- `pnpm dev`: start on local http://localhost:3000
