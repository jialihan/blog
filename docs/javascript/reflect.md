## Reflect Object in JavaScript

#### 1.[ Preface](#p1)

#### 2.[ Reflect.ownKeys()](#p2)

#### 3.[ Reflect.get()](#p3)

#### 4.[ Reflect.set()](#p4)

#### 5.[ Reflect.has()](#p5)

#### 6.[ Reflect.deleteProperty()](#p6)

#### 7.[ Reflect.apply()](#p7)

#### 8.[ Reflect.defineProperty()](#p8)

<div  id="p1"  />

### 1. Preface

[Reflect - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Reflect)

**`Reflect`** is a built-in object that provides methods for interceptable JavaScript operations. The methods are the same as those of [proxy handlers](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy/Proxy). `Reflect` is not a function object, so it's not constructible.

Unlike most global objects, `Reflect` is not a constructor. You cannot use it with a [`new` operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/new) or invoke the `Reflect` object as a function. All properties and methods of `Reflect` are static (just like the [`Math`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math) object).

The `Reflect` object provides the following static functions which have the same names as the [proxy handler methods](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy/Proxy).

<div  id="p2"  />

### 2. Reflect.ownKeys()

The static **`Reflect.ownKeys()`** method returns an array of the `target` object's own property keys. But it does NOT return the prototype chain's props.

```
const obj = {
	age: 19,
	calcAge() {
		return this.age-1;
	}
}
Reflect.ownKeys(obj); // ['age', 'calcAage'];
```

<div  id="p3"  />

### 3. Reflect.get()

Syntax:

```
Reflect.get(target, propertyKey)
Reflect.get(target, propertyKey, receiver)
```

For example:

```const obj = {
	age: 19,
	calcAge() {
		return this.age-1;
	}
}
Reflect.get(obj, 'age'); // 19
```

<div  id="p4"  />

### 4. Reflect.set()

The static **`Reflect.set()`** method works like setting a property on an object.
Syntax:

```
Reflect.set(target, propertyKey, value)
Reflect.set(target, propertyKey, value, receiver)
```

For example:

```
Reflect.set(obj, 'age', 18);
obj.age; // 18
```

<div  id="p5"  />

### 5. Reflect.has()

The static **`Reflect.has()`** method works like the [`in` operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/in) as a function. It also returns the property under prototype chain.

Syntax:

```
Reflect.has(target, propertyKey)
```

For example:

```
const a = {foo: 123}
const b = {__proto__: a}
const c = {__proto__: b}
// The prototype chain is: c -> b -> a
Reflect.has(c, 'foo') // true
```

<div  id="p6"  />

### 6. Reflect.deleteProperty()

The static **`Reflect.deleteProperty()`** method allows to delete properties. It is like the [`delete` operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/delete) as a function.

Syntax:

```
Reflect.deleteProperty(target, propertyKey)
```

For example:

```
const obj = {
	age: 19,
	calcAge() {
		return this.age-1;
	}
}
Reflect.deleteProperty(obj, 'age');
```

<div  id="p7"  />

### 7. Reflect.apply()

The static **`Reflect.apply()`** method calls a target function with arguments as specified.

In ES5, you typically use the [`Function.prototype.apply()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply) method to call a function with a given `this` value and `arguments` provided as an array (or an [array-like object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Indexed_collections#working_with_array-like_objects)).

```
Function.prototype.apply.call(Math.floor, undefined, [1.75]);
```

With `Reflect.apply()` this becomes less verbose and easier to understand.

Syntax:

```
Reflect.apply(target, thisArgument, argumentsList)
```

For example:

```
const obj = {
	age: 19,
	calcAge() {
		return this.age-1;
	}
}
const obj2 = {
    age: 18,
}

Reflect.apply(obj.calcAge, obj2, []); // 17
Reflect.apply(obj.calcAge, obj2); // 17, with no params
// equals
obj.calcAge.apply(obj2, []); // 17
obj.calcAge.apply(obj2); // with no params
// equals
Function.prototype.apply.call(obj.calcAge, obj2, []);
```

<div  id="p8"  />

### 8. Reflect.defineProperty()

The static **`Reflect.defineProperty()`** method is like [`Object.defineProperty()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty) but returns a [`Boolean`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean).

Syntax:

```
Reflect.defineProperty(target, propertyKey, discriptor_attributes)
```

A [discriptor](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty) attributes including:

- configurable
- enumerable
- value
- writable
- get: defaults to undefined
- set: defaults to undefined

For example:

```
const obj = {
	age: 19,
	calcAge() {
		return this.age-1;
	}
}

Reflect.defineProperty(obj, 'name', {value: 'jelly', writable: false});
```
