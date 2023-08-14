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

### 2.2 vue dev tools (chrome extension)

link: https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd

### 2.3 data() method

A function that returns the **initial reactive state** for the component instance. The function is expected to return a **plain JavaScript object**, which will be made reactive by Vue. ([doc link](https://vuejs.org/api/options-state.html#data))

```
Vue.createApp({
  data() {
    return {
      firstName: "Jelly",
      lastName: "Test"
    };
  }
}).mount("#app");
```

Use VUE tools to check data is populated into vue elements:
![image](../assets/vue-section2-2_3_01.png)

**!!!复习**： shorthand syntax to defind a method in an `object / class` ([mdn-doc: method definitions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Method_definitions)):

```
const obj = {
  foo: function () {
    // old way
  },
  bar() {
    // shorthand syntax
  },
};
```

### 2.4 js expressions in template

- Expressions([mdn doc](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_operators)) are allowed in template in vue selected elements.
  ```
    <div id="app">
      <!-- js expressions inside templates in selected elements by vue -->
      {{2 + 2 }}
    </div>
  ```
- text interpolation: all data props are exposed directly by their names -[] [Template Syntx - vue doc](https://vuejs.org/guide/essentials/template-syntax.html#text-interpolation)
  ```
    <div id="app">
      <!-- all data props are exposed directly by their names -->
      {{firstName}}
    </div>
  ```

### 2.5 multiple vue instances

1. same `.app` selector, but **VUE instance will only be mounted to one element**. If the selector find multiple elements in the `mount()` function, it will return the **1st element it finds**.

```
<div class="app">
    {{firstName}} {{lastName}}
</div>
<!-- 2nd instances of mounted element -->
<div class="app">
    {{firstName}} {{lastName}}
</div>
```

![image](../assets/vue-section2-2_5_01.png)

2. Two vue instances for each root element - allowed ✅

```javascript
Vue.createApp({
  data() {
    return {
      firstName: "Jelly",
      lastName: "Test"
    };
  }
}).mount(".app");

Vue.createApp({
  data() {
    return {
      firstName: "Jelly2",
      lastName: "Test2"
    };
  }
}).mount(".app2");
```

```vue
<div class="app">
    {{firstName}} {{lastName}}
</div>
<!-- 2nd instances of mounted element -->
<div class="app2">
    {{firstName}} {{lastName}}
</div>
```

![image](../assets/vue-section2-2_5_02.png)

### 2.6 access the instance data()

1. vue proxies ([doc](https://vuejs.org/api/options-state.html#data)):
   The component instance also proxies all the properties found on the data object, so `this.foo` will be equivalent to `this.$data.foo`.

- What is a proxy? - "a figure that can be used to represent the value of something in a calculation".
  ![image](../assets/vue-section2-2_6_01.png)
