### 2.1 use Vue from CDN

1. [doc](https://vuejs.org/guide/quick-start.html#using-vue-from-cdn): where all top-level APIs are exposed as properties on the global `Vue` object.

```
  <body>
    <div id="app"><p>Hello world</p></div>

    <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
    <script src="app.js"></script>
  </body>
```

2. `createApp`

```
Vue.createApp({}).mount("#app");
```

Open the HTML on local machine, you will see the below image:
![image](../assets/vue_section_2-1-1.png)
