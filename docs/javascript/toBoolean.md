## Double negation (!!) in javascript

### 1. How it works?

The first `!` negates it once, converting values like so:

- undefined to true
- null to true
- +0 to true
- -0 to true
- '' to true
- NaN to true
- false to true
  All other expressions to false

Then the other `!` negates it again, so finally:

- `!!undefined` to false
- `!!null` to false
- `!!+0` to false
- `!!-0` to false
- `!!''` to false
- `!!NaN` to false
- `!!false` to false

### 2. What is the purpose?

Then the other `!` negates it again. A concise cast to boolean, exactly equivalent to [ToBoolean](https://es5.github.io/#x9.2) simply because `!` is [defined as its negation](https://es5.github.io/#x11.4.9).

So it's used convert a "truethy" \"falsy" value to a boolean.

### 3. References

https://stackoverflow.com/questions/10467475/double-negation-in-javascript-what-is-the-purpose
