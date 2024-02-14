## VUE - Custom Directive

[Custom directives](https://vuejs.org/guide/reusability/custom-directives) allow you to modify a small portion of an element without having to create a whole component. It can be registered globally or locally.

### 1.[Directive Hooks](https://vuejs.org/guide/reusability/custom-directives#directive-hooks)

lifecycle functions(all are optional):

- `beforeMount` - called when directives is first bound to the element and before parent component is mounted.
- `mounted` - callend when the directive is mounted to the document.
- `beforeUpdate` - called before the directive is updated.
- `updated` - called after the directive is updated.
- `beforeUnmount` - before a directive is unmounted from the document
- `unmounted` - called after a directive is unmounted from the document

### 2. [Hook arguments](https://vuejs.org/guide/reusability/custom-directives#hook-arguments)

- `el`: the element the binding sits on
- `binding` - an object which contains the arguments that are passed into the hooks.
- `vnode`: allows you to refer directly to the node in the virtual DOM if you need to.
- `prevNode`: the previous **vnode** object before the directive was updated. (**only** apply to **beforeUpdate & updated** hooks)

### 3. register a icon globally

It is also common to globally register custom directives at the app level:

```js
const app = createApp({});

// make v-focus usable in all components
app.directive("focus", {
  /* ... */
});
```

### 4. pass values to directives

```html
<div v-icon="'headphones-alt'"></div>
```

in js code:

```js
  beforeMount(el, binding) {
    const iconClass = `fa fa-${binding.value} float-right text-green-400 text-xl`;
    el.innerHTML += `<i class="${iconClass}"></i>`;
  },
```

`binding.arg`:

```
if (binding.arg === "full") {}
// consume in template:
v-icon:full="........"
```

### 5.Directive modifiers

Use the modifiers in v-directive:

```html
<div v-icon.right.yellow="'headphones-alt'"></div>
```

Implementation in JS code:

```js
// check modifiers if exists
if (binding.modifiers.right) {
  iconClass += " float-right";
}
if (binding.modifiers.yellow) {
  iconClass += " text-yellow-400";
} else {
  iconClass += " text-green-400";
}
```

### 6. register directive locally

Pass an object to the directives:

```vue
<div
    v-icon-secondary="{ icon: 'headphones-alt', right: true }"
>
```

Implement in JS code:

```
// access in JS file
binding.value.icon
binding.value.right
```

import in the local component.js file:

```js
import IconSecondary from "@/directives/icon-secondary";
export default {
  directives: {
    "icon-secondary": IconSecondary,
  },
  // ...
};
```
