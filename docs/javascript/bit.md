## Number and Bitwise Operator in JavaScript

### 1. 64-bit double precision

The JavaScript `Number` type is a [double-precision 64-bit binary format IEEE 754](https://en.wikipedia.org/wiki/Floating-point_arithmetic) value, like `double` in Java or C#.

A `Number` only keeps about **17** decimal places of precision;

The maximum safe integer in JavaScript (`2^53 - 1`).

MDN doc: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number

### 2. Bitwise operator 32 bit Integer

> ... operands are converted to thirty-two-bit integers and expressed by a series of bits (zeros and ones). Numbers with more than **32** bits get their most significant bits discarded.

JavaScript uses a 32-bit layout for bitwise operations. Therefore, the range is -`-2^31` to `2^31-1` (equivalent to `-2147483648` to `2147483647`).

**Rules** to find the negative's two's complement:
There are many ways to compute two’s complement of a number. Here’s a [one](https://stackoverflow.com/questions/1049722/what-is-2s-complement) I like to use:

```text
1. Convert the magnitude of the number to binary.
2. If the number is positive, then we are done.
3. If the number is negative:
   a. Find the one's complement of binary by inverting 0's and 1's.
   b. Add 1 to the one's complement.
```

**"2^31 - 1"**

```text
 2147483647 => 01111111111111111111111111111111
-----------------------------------------------
two's complement 01111111111111111111111111111111
-----------------------------------------------
```

**"-2^31"**

```text
 2147483648 => 10000000000000000000000000000000
 Complement => 01111111111111111111111111111111
                                             +1
-----------------------------------------------
two's complement 10000000000000000000000000000000
-----------------------------------------------
```

**Edge case:** cannot use more than 32 bits in operators

```
console.log(1<<32); // output: 0
```

It is because the 2’s complement of 232 is zero in 32-bits. We shift zero by zero bits and end up with zero.

```text
    4294967296 => 100000000000000000000000000000000 (33 bits)
    4294967296 =>  00000000000000000000000000000000 (32 bits)
-------------------------------------------------------------
two's complement =>  00000000000000000000000000000000 (32 bits)
-------------------------------------------------------------
```

### 3. count one bits in number - JS

Doc: https://www.geeksforgeeks.org/count-set-bits-in-an-integer/

So if we subtract a number by 1 and do it bitwise & with itself `(n & (n-1))`, we unset the rightmost set bit. If we do `n & (n-1)` in a loop and count the number of times the loop executes, we get the set bit count.

```
/* Function to get no of set
bits in binary representation
of passed binary no. */

function countSetBits(n) {
	var count = 0;
	while (n > 0) {
		n &= (n - 1);
		count++;
	}
	return  count;
}
```
