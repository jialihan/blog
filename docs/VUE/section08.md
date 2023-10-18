## Section 8 - build a app - quiz

### 8.1 static asset handling

Vue doc: [link](https://vitejs.dev/guide/assets.html)

- assets folder
- public folder

For example: we can import external css file

```js
// main.js
import "./assets/main.css";

import { createApp } from "vue";
import App from "./App.vue";

createApp(App).mount("#app");
```

### 8.2 Rendering the components

复习:

- `v-for， :key`: to render a list, must have a unique key (section 2.21)
- `v-if, v-else`: to control show or hide

### 8.3 communication on components

复习：

- `this.$emit('topicName', ...args)`: section 6.9, and consume the event: `@eventName="handlerFunction"`
- section 2.18: binding css styles attribute: `:style="{ ... }"`

### 8.4 animations

- add animation: `<transition-group>` on the `v-for` list items, reference in section 7.7 - animate a list
- use `computed props`
