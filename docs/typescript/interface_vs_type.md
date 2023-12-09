## What is the difference between Interface and Type?

Reference: [doc](https://blog.bitsrc.io/type-vs-interface-in-typescript-cf3c00bc04ae).

- **Interface**: interfaces are essentially a means of describing the shapes of data, like an object.
- **Type**: the definitions of data types, like primitive, intersection, union, tuple, or **different types**.

### When to use?

#### Use interfaces when:

- A new object or an object method needs to be defined.
- You wish to benefit from declaration merging.

#### Use types when:

- You need to define a primitive-type alias
- Defining tuple types
- Defining a union
- You must create functions and attempt to overload them in object types through composition.
- Requiring the use of mapped types
