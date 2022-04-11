## React 18 migration

### 1. how to upgrade my code base to react 18?

If you create your react app with the `npx create-react-app <your app name>` , you will get the latest 18 version already.

Or you can manually upgrade on your existing react app through npm package for `react` and `react-dom` in 18 version above.

```bash
npm install react@18 react-dom@18
```

How to fix the console warning?

![image](../assets/react-18-01.png)

Original code:

```jsx
import ReactDOM from "react-dom";

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById("root")
);
```

Change to:

```jsx
import ReactDOM from "react-dom/client";

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

### 2. What are new features in React 18?

Official doc link: [What's New in React 18](https://reactjs.org/blog/2022/03/29/react-v18.html#whats-new-in-react-18)

#### 2.1 Automatic Batching

#### 2.2 Transitions

#### 2.3 Suspense - SSR in React

React Conf 2021 talk: [Streaming Server Rendering with Suspense](https://www.youtube.com/watch?v=pj5N-Khihgc&list=PLNG_1j3cPCaZZ7etkzWA7JfdmKWT0pMsa&index=3)
