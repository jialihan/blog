## BFE 26. implement Object.assign()

#### 1. [Problem](#question1)

#### 2. [Wrong Solution - Object.getOwnPropertyDescriptors()](#question2)

#### 3. [Correct Solution - my own solution](#question3)

#### 4. [Reference and Links](#question4)

<div id="question1" />

### I. Problem

_The `Object.assign()` method copies all enumerable own properties from one or more source objects to a target object. It returns the target object._ (source: [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign))

It is widely used, Object Spread operator actually is internally the same as `Object.assign()` ([source](https://github.com/tc39/proposal-object-rest-spread/blob/master/Spread.md)). Following 2 lines of code are totally the same.

```js
let aClone = { ...a };
let aClone = Object.assign({}, a);
```

This is an easy one, could you implement `Object.assign()` with your own implementation ?

_note_

**Don't use Object.assign() in your code** It doesn't help improve your skills

<div id="question2" />

### II. Wrong Solution - Object.getOwnPropertyDescriptors()

Wrong code:

```js
function objectAssign(target, ...sources) {
  if (target === null || target === undefined) {
    throw new Error("Not an object");
  }

  if (typeof target !== `object`) {
    target = new target.__proto__.constructor(target);
  }

  for (const source of sources) {
    if (source === null || source === undefined) {
      continue;
    }

    Object.defineProperties(target, Object.getOwnPropertyDescriptors(source));
    // // NOT necessary: it already includes symboled keys
    // for (const symbol of Object.getOwnPropertySymbols(source)) {
    // target[symbol] = source[symbol]
    // }
  }
  return target;
}
```

**do NOT use:**

```javascript
Object.defineProperties(target, Object.getOwnPropertyDescriptors(source));
```

**Error Case:**

```
objectAssign({}, [1,2]);   // {0:1, 1:2, length:2}
Object.assign({}, [1,2]);  // {0:1, 1:2}
```

We can see ["getOwnPropertyDescriptors()"](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptors) return ALL props even those with prop `enumerable === false`, then you can see some unnecessary prop, eg: "length" also included in assigned object.

Yes, we can fix it, we need an extra function to filter those ONLY `"enumerable=== true"` descriptors:

```js
function filteredDescriptors(obj) {
  // filter the props with enumerable: true
  // TODO:
  // regular keys & symbol keys
}
```

<div id="question3" />

### III. Correct Solution - my own solution

Please see my **explanation** to the important two **annotations** here in code:

- #1: Bug: omit the **Symboled keys** in object
- #2: Bug: throw Error **when the original prop is NOT writable**

**JS Solution**

```javascript
/**
 * @param {any} target
 * @param {any[]} sources
 * @return {object}
 */
function objectAssign(target, ...sources) {
  if (target == null || target === undefined) {
    throw new Error("invalid target object!");
  }
  var obj = new Object(target);
  for (var src of sources) {
    if (src === null || src === undefined) {
      continue;
    }
    // #1
    var keys = [
      ...Object.keys(src),
      ...Object.getOwnPropertySymbols(src)
    ].filter((item) => Object.getOwnPropertyDescriptor(src, item).enumerable);
    for (var k of keys) {
      // #2
      if (
        Object.getOwnPropertyDescriptor(obj, k) &&
        Object.getOwnPropertyDescriptor(obj, k).writable === false
      ) {
        throw new Error("this prop can NOT be written!");
      }
      obj[k] = src[k];
    }
  }
  return obj;
}
```

#### 3.1 should include ALL keys as well as Symboled keys

Failed test case:

```
const key = Symbol('key')
 const a = {
   [key]: 3,
   b: 4
};
const target = {};
console.log(objectAssign(target, a));
```

Because `Obje**strong text**ct.keys(obj)` will NOT return "Symbol keys".

Fix:
use `Object.getOwnPropertySymbols(source)`, but it return an **array** of symbol keys.

Docs: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertySymbols

#### 3.2 Prop is NOT writable in target

Failed test case:

```
const target = Object.defineProperty({}, 'foo', {
    value: 1,
    writable: false
  }); // target.foo is a read-only property

  expect(() => objectAssign(target,
                            { bar: 2 },
                            { foo2: 3, foo: 3, foo3: 3 },
                            { baz: 4 })
        ).toThrow()
  expect(target).toEqual({bar: 2, foo2: 3})
```

**Fixed:** manually check this property in its descriptor

<div id="question4" />

### IV. Reference and Links

- Object properties in JavaScript: https://2ality.com/2012/10/javascript-properties.html
- "Object.getOwnPropertyDescriptor()": https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptor
- "Object.getOwnPropertyDescriptor**s**()": https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptors
- "Object.getOwnPropertySymbols()": https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertySymbols
- **TODO**: Know more about **Reflect in JS**: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Reflect
