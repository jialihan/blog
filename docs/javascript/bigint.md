## BigInt - JavaScript - ECMAScript 2020 - ES11

### 1. BigInt
* BigInt is a built-in object that provides a way to represent whole numbers larger than 253 - 1, which is the largest number JavaScript can reliably represent with the Number primitive and represented by the Number.MAX_SAFE_INTEGER constant. 

* BigInt can be used for arbitrarily large integers.

* Usage Example:
```
const previouslyMaxSafeInteger = 9007199254740991n
const alsoHuge = BigInt(9007199254740991)
const hugeHex = BigInt("0x1fffffffffffff")
const hugeBin = BigInt("0b11111111111111111111111111111111111111111111111111111")
```

### 2. MAX_SAFE_INTEGER

The **Number.MAX_SAFE_INTEGER** constant represents the maximum safe integer in JavaScript `(2^53 - 1)`, a value of `9007199254740991`.

Usage Example:

```
const x = Number.MAX_SAFE_INTEGER + 1;
const y = Number.MAX_SAFE_INTEGER + 2;

console.log(Number.MAX_SAFE_INTEGER);
// expected output: 9007199254740991

console.log(x);
// expected output: 9007199254740992

console.log(x === y);
// expected output: true
```

### 3. DOCS

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigInt

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/MAX_SAFE_INTEGER


