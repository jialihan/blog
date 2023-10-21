## Section 10.11 Vue Slots

Doc: [link](https://vuejs.org/guide/components/slots.html#slots)

### 1. Slot content and outlet

Parent Component provide the `slot content`

```jsx
// in parent template
<MyComponent>
  Click me! <!-- slot content -->
</MyComponent>
```

Child component create the `outlet`

```html
<button>
  <slot></slot>
  <!-- slot outlet -->
</button>
```

The `<slot>` element is a slot outlet that indicates where the parent-provided slot content should be rendered. The Result is equal to:

```html
<button>Click me!</button>
```

### 2. Fallback/default content in slot

doc: [link](https://vuejs.org/guide/components/slots.html#fallback-content)

In child component, if we want to define a default/fallback value of the `<slot>`:

```jsx
<button>
  <slot>
    Submit <!-- fallback content -->
  </slot>
</button>
```

So if parent doesn't provide any content, the result will fallback to the default value in the `<slot>`:

```jsx
// parent
<MyComponent></MyComponent>
// result
<button> Submit</button>
```

### 3. Named Slot

doc: [link](https://vuejs.org/guide/components/slots.html#named-slots)
Child Component: define the named slot

```html
<div class="container">
  <header>
    <!-- We want header content here -->
    <slot name="header"></slot>
  </header>
  <p>Main Content</p>
  <footer>
    <!-- We want footer content here -->
    <slot name="footer"></slot>
  </footer>
</div>
```

> Note: A `<slot>` outlet without name implicitly has the **name "default"**.

Parent Component: provide the header & footer content
**Shorthand**:

- `#default` equals to `v-slot="props"`
- `#header` equals to `v-slot:header`
- `#footer` equals to `v-slot:footer`

```jsx
<MyComponent>
  <template #header>
    <h1>my header</h1>
  </template>
  <template #footer>
    <p>my footer</p>
  </template>
</Mycomponent>
```

The Result is:

```html
<div class="container">
  <header>
    <!-- We want header content here -->
    <h1>my header</h1>
  </header>
  <p>Main Content</p>
  <footer>
    <!-- We want footer content here -->
    <p>my footer</p>
  </footer>
</div>
```

### 4. Share Child component state/data from Slot

doc: [link](https://vuejs.org/guide/components/slots.html#scoped-slots)

However, there are cases where it could be useful if a slot's content can make use of data from both the parent scope and the child scope. To achieve that, we need a way for the child to pass data to a slot when rendering it.

4.1 Child component: use `v-bind:prop1` to yield out each property & value

```html
<!-- child <MyComponent> -->
<div>
  <slot :age="12" :name="Alice"></slot>
  <slot name="header" :message="test"></slot>
</div>
```

4.2 Parent component: receive props from a **single default slot**
use the `v-slot` directly on the child component tag:

```vue
<MyComponent v-slot="props">
  {{ props.age }} {{ props.name }}
</MyComponent>
```

Or we can also use the **destructure** of the props object:

```vue
<MyComponent v-slot="{ age, name }">
  {{ age }} {{ name }}
</MyComponent>
```

4.3 Parent component: receive props from a **named slot** - [doc](https://vuejs.org/guide/components/slots.html#scoped-slots)

> Note: the named slot is defined as: `<slot name="header" :message="test"></slot>`.

❌**Wrong way** of receiving slot props!!!

```vue
<!-- Wrong: cannot get props on the child component tag -->
<MyComponent v-slot="{ message }">
  <template #header>
    <p>{{ message }}</p>
  </template>
</MyComponent>
```

✅**Correct way**: using an **explicit** `<template>` tag for the named slot helps to make it clear that the `{ message }` prop is only available in current specific slot.

```vue
<MyComponent>
  <template #header v-slot="{ message }">
    <p>{{ message }}</p>
  </template>
</MyComponent>
```

#### 4.4 how to share an data object rather than a single prop?

> TIP: we are using `v-bind` to pass **an object** as slot props. Please reference the issue at github: [vue/#2114](https://github.com/vuejs/vue/issues/2114).

For example: in Child Component:

```js
<ul>
  <li v-for="item in items">
    <slot name="item" v-bind="item"></slot>
  </li>
</ul>
```

In Parent Component to receive the object props: `#item="{ name, age, message }"`

```vue
// parent component
<MyComponent>
  <template #item="{ name, age, message }">
     <h1> {{ name }} {{ likes }}  {{message}}</h1>
  </template>
</MyComponent>
```
