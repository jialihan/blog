# Operator precedence in JavaScript


## Table

**Docs:** [mdn - Operator Precedence](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence)

| Precedence | Operator type |  Associativity |
|--|--|--|
|  12| [Less Than (<)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Less_than) <br> [Less Than Or Equal (<=)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Less_than_or_equal) <br> [Greater Than (>)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Greater_than)  <br> [Greater Than Or Equal (>=)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Greater_than_or_equal) | left-to-right  | 
| 11 | [Equality (`==`)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Equality) <br> [Inequality (!=)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Inequality) <br> [Strict Equality (`===`)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Strict_equality) <br> [Strict Inequality (!==)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Strict_inequality) |  left-to-right   |
| 3 | [Assignment(=, +=...)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators#assignment_operators) | right-to-left | 


## Examples

#### 1. Equality Related
All is `left-to-right` evaluation
```js
console.log(0  ==  1  ==  2); // (false == 2) -> false
console.log(2  ==  1  ==  0); // (false == 0) -> true
```

#### 2. Inequality  Related
All is `left-to-right` evaluation
```js
console.log(0  <  1  <  2); // (true < 2) -> (1 < 2) -> true
console.log(1  <  2  <  3); // (true < 3) -> (1 < 3) -> true
console.log(2  >  1  >  0); // (true > 0) -> (1 > 0) -> true
console.log(3  >  2  >  1); // (true > 1) -> (1 < 1) -> false
```






